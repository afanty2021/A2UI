# Agent示例

<cite>
**本文档中引用的文件**   
- [a2ui_examples.py](file://samples/agent/adk/contact_lookup/a2ui_examples.py)
- [agent.py](file://samples/agent/adk/contact_lookup/agent.py)
- [tools.py](file://samples/agent/adk/contact_lookup/tools.py)
- [prompt_builder.py](file://samples/agent/adk/contact_lookup/prompt_builder.py)
- [a2ui_schema.py](file://samples/agent/adk/contact_lookup/a2ui_schema.py)
- [agent.py](file://samples/agent/adk/restaurant_finder/agent.py)
- [a2ui_examples.py](file://samples/agent/adk/restaurant_finder/a2ui_examples.py)
- [tools.py](file://samples/agent/adk/restaurant_finder/tools.py)
- [prompt_builder.py](file://samples/agent/adk/restaurant_finder/prompt_builder.py)
- [agent.py](file://samples/agent/adk/orchestrator/agent.py)
- [subagent_route_manager.py](file://samples/agent/adk/orchestrator/subagent_route_manager.py)
- [agent.py](file://samples/agent/adk/rizzcharts/agent.py)
- [component_catalog_builder.py](file://samples/agent/adk/rizzcharts/component_catalog_builder.py)
- [tools.py](file://samples/agent/adk/rizzcharts/tools.py)
- [rizzcharts_catalog_definition.json](file://samples/agent/adk/rizzcharts/rizzcharts_catalog_definition.json)
</cite>

## 目录
1. [联系人查找](#联系人查找)
2. [餐厅查找](#餐厅查找)
3. [协调器](#协调器)
4. [RizzCharts](#rizzcharts)

## 联系人查找

`contact_lookup`示例展示了如何利用A2UI生成联系人搜索表单和结果列表。该示例的核心是`a2ui_examples.py`文件中的`generate_contact_search_ui`方法，它定义了用于生成UI的JSON模板。

`a2ui_examples.py`文件包含多个预定义的UI示例，如`CONTACT_LIST_EXAMPLE`和`CONTACT_CARD_EXAMPLE`，这些示例定义了如何渲染联系人列表和单个联系人卡片。`CONTACT_LIST_EXAMPLE`使用`List`组件和`template`属性来动态生成联系人列表，其中`dataBinding`指向数据模型中的`/contacts`路径。每个联系人项使用`Card`组件进行渲染，包含头像、姓名、职位和一个"查看"按钮。`CONTACT_CARD_EXAMPLE`则用于显示单个联系人的详细信息，包括姓名、职位、团队、日历、位置、电子邮件和电话号码，并提供"关注"和"发送消息"按钮。

`agent.py`文件中的`ContactAgent`类负责处理用户查询。`_build_agent`方法根据`use_ui`参数决定是否使用UI提示。`stream`方法实现了核心逻辑：它首先调用LLM，然后验证返回的A2UI JSON是否符合`a2ui_schema.py`中定义的模式。如果验证失败，它会进行重试。`prompt_builder.py`文件中的`get_ui_prompt`函数构建了LLM的系统提示，其中包含了UI指令、规则、示例和A2UI模式。`tools.py`文件中的`get_contact_info`工具函数用于从`contact_data.json`文件中检索联系人信息，并根据查询参数进行过滤。

**最佳实践总结**：
- 使用预定义的JSON模板来确保UI的一致性。
- 在`prompt_builder.py`中明确指定UI生成规则和示例。
- 在`agent.py`中实现A2UI JSON的验证和重试逻辑，以提高可靠性。
- 使用`dataModelUpdate`消息来绑定动态数据。

**Section sources**
- [a2ui_examples.py](file://samples/agent/adk/contact_lookup/a2ui_examples.py#L1-L163)
- [agent.py](file://samples/agent/adk/contact_lookup/agent.py#L1-L293)
- [tools.py](file://samples/agent/adk/contact_lookup/tools.py#L1-L69)
- [prompt_builder.py](file://samples/agent/adk/contact_lookup/prompt_builder.py#L1-L119)
- [a2ui_schema.py](file://samples/agent/adk/contact_lookup/a2ui_schema.py#L1-L793)

## 餐厅查找

`restaurant_finder`示例展示了如何结合工具调用和A2UI生成餐厅预订表单。该示例通过`tools.py`中的`get_restaurants`工具函数获取餐厅数据，并使用A2UI生成相应的UI。

`a2ui_examples.py`文件定义了多个UI示例，包括`SINGLE_COLUMN_LIST_EXAMPLE`、`TWO_COLUMN_LIST_EXAMPLE`、`BOOKING_FORM_EXAMPLE`和`CONFIRMATION_EXAMPLE`。`SINGLE_COLUMN_LIST_EXAMPLE`和`TWO_COLUMN_LIST_EXAMPLE`用于显示餐厅列表，区别在于布局。`BOOKING_FORM_EXAMPLE`展示了如何使用`TextField`和`DateTimeInput`等表单组件来收集用户输入。`dataModelUpdate`消息将表单字段（如`partySize`和`reservationTime`）绑定到数据模型，当用户提交表单时，这些值会作为上下文传递给`submit_booking`动作。

`agent.py`文件中的`RestaurantAgent`类与`ContactAgent`类似，但其`AGENT_INSTRUCTION`和`get_ui_prompt`函数针对餐厅查找场景进行了定制。`get_restaurants`工具函数从`restaurant_data.json`文件中获取数据，并根据`cuisine`、`location`和`count`参数进行过滤。

**最佳实践总结**：
- 为不同的数据量（单列 vs 双列）提供不同的UI模板。
- 使用`TextField`和`DateTimeInput`等组件创建交互式表单。
- 利用`Button`组件的`action.context`属性在表单提交时传递数据。
- 在系统提示中明确说明如何根据用户意图选择正确的UI模板。

**Section sources**
- [agent.py](file://samples/agent/adk/restaurant_finder/agent.py#L1-L304)
- [a2ui_examples.py](file://samples/agent/adk/restaurant_finder/a2ui_examples.py#L1-L187)
- [tools.py](file://samples/agent/adk/restaurant_finder/tools.py#L1-L56)
- [prompt_builder.py](file://samples/agent/adk/restaurant_finder/prompt_builder.py#L1-L800)

## 协调器

`orchestrator`示例展示了多代理协作机制，其中主Agent通过A2UI界面协调子Agent并路由任务。

`agent.py`文件中的`OrchestratorAgent`类是核心。`build_agent`方法通过`subagent_urls`参数动态创建`RemoteA2aAgent`实例。它使用`A2ACardResolver`从远程URL获取子Agent的Agent Card，并将其转换为ADK可以理解的格式。`A2UIMetadataInterceptor`是一个客户端调用拦截器，它在向子Agent发送请求时添加A2UI扩展头和客户端功能，确保子Agent知道如何生成A2UI响应。

`programmtically_route_user_action_to_subagent`方法是关键的路由逻辑。它在LLM调用之前执行，检查用户操作（`userAction`）中的`surfaceId`，并使用`SubagentRouteManager`根据`surfaceId`查找目标子Agent的名称。如果找到匹配项，它会直接返回一个`transfer_to_agent`函数调用，从而绕过LLM的决策过程，实现程序化路由。

**最佳实践总结**：
- 使用`RemoteA2aAgent`和`A2ACardResolver`来集成远程子Agent。
- 利用`ClientCallInterceptor`在请求中注入元数据（如A2UI头）。
- 实现`before_model_callback`来进行程序化路由，提高效率和准确性。
- 通过`SubagentRouteManager`管理`surfaceId`到子Agent的映射关系。

**Section sources**
- [agent.py](file://samples/agent/adk/orchestrator/agent.py#L1-L189)
- [subagent_route_manager.py](file://samples/agent/adk/orchestrator/subagent_route_manager.py)

## RizzCharts

`rizzcharts`示例重点讲解了如何定义包含图表和地图的自定义组件目录，并生成复杂的可视化UI。

`component_catalog_builder.py`文件中的`ComponentCatalogBuilder`类负责构建自定义组件目录。`load_a2ui_schema`方法是核心，它接收客户端的UI功能（`client_ui_capabilities`），并根据`supported_catalog_uris`选择合适的目录。如果客户端支持`RIZZCHARTS_CATALOG_URI`，则加载本地的`rizzcharts_catalog_definition.json`；否则，回退到标准目录。该方法将选定的目录合并到基础A2UI模式中，形成一个包含自定义组件（如`Chart`和`GoogleMap`）的完整模式。

`rizzcharts_catalog_definition.json`文件定义了自定义组件。`Chart`组件支持`doughnut`和`pie`类型，并接受分层数据结构（`chartData`），支持钻取功能。`GoogleMap`组件允许在地图上显示带有自定义样式的标记（`pins`），包括背景色、边框色和图标色。

`agent.py`文件中的`rizzchartsAgent`类使用`get_instructions`方法动态生成系统提示。该提示根据`readonly_context.state`中的`A2UI_CATALOG_URI_STATE_KEY`选择正确的示例（`rizzcharts_catalog`或`standard_catalog`）。`build_agent`方法注册了`get_store_sales`、`get_sales_data`和`A2uiToolset`工具。`A2uiToolset`工具用于发送A2UI JSON到客户端。`get_instructions`方法中的工作流指导LLM：分析请求意图（图表或地图），调用相应的工具获取数据，选择正确的JSON示例，更新`surfaceId`和标题，然后调用`send_a2ui_json_to_client`工具。

**最佳实践总结**：
- 使用`ComponentCatalogBuilder`动态构建和合并组件目录。
- 在自定义组件目录中定义专用的可视化组件（如图表和地图）。
- 在系统提示中提供完整的JSON示例，指导LLM生成正确的A2UI消息。
- 为每个请求生成唯一的`surfaceId`以避免冲突。
- 利用`A2uiToolset`工具简化A2UI消息的发送。

**Section sources**
- [agent.py](file://samples/agent/adk/rizzcharts/agent.py#L1-L153)
- [component_catalog_builder.py](file://samples/agent/adk/rizzcharts/component_catalog_builder.py#L1-L88)
- [tools.py](file://samples/agent/adk/rizzcharts/tools.py#L1-L96)
- [rizzcharts_catalog_definition.json](file://samples/agent/adk/rizzcharts/rizzcharts_catalog_definition.json#L1-L125)