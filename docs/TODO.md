# Version 0.0.1
0. 该版本暂时不添加网络逻辑。
1. 完成词汇数据库、题单数据库、历史记录数据库的实现。
2. 完成单词拼写题的错误分析与逻辑实现，并将它们保存至UI。
3. 完成题单逻辑设计。
4. 完成主界面UI、Learn页面UI、题单添加页面UI的呈现。
项目构成：Isar数据库、riverpod状态管理。

文件项目结构：
lib/
├─ main.dart                          # App入口 (ProviderScope)
├─ app/
│  ├─ app.dart                        # MaterialApp / GoRouter 包装 ProviderScope 外壳
│  ├─ routing/
│  │  └─ app_router.dart              # 路由配置 (GoRouter)
│  └─ theme/
│     └─ app_theme.dart               # 主题、颜色、字体
│
├─ core/
│  ├─ configs/
│  │  └─ app_config.dart
│  ├─ constants/
│  │  └─ db_constants.dart
│  ├─ error/
│  │  ├─ exceptions.dart
│  │  └─ failures.dart
│  ├─ utils/
│  │  ├─ text_utils.dart             # normalize, strip punctuation
│  │  ├─ levenshtein.dart
│  │  └─ phonetics.dart
│  └─ services/
│     └─ local_notifications.dart
│
├─ data/
│  ├─ datasources/
│  │  └─ local/
│  │     ├─ app_database.dart  # 初始化 Isar，暴露 Future<Isar> init()、Isar get instance，处理 schemaVersion & migrations
│  │     ├─ local_datasources_providers.dart  # 用 Riverpod 提供 app database 与 local data source 的 Provider/Provider.family
│  │     ├─ word_local_datasource.dart
│  │     ├─ question_local_datasource.dart
│  │     └─ history_local_datasource.dart
│  ├─ mappers/
│  │  ├─ word_mapper.dart
│  │  └─ question_mapper.dart
│  ├─ models/
│  │  └─ isar/
│  │     ├─ word_model.dart
│  │     └─ question_model.dart
│  └─ repositories/
│     ├─ word_repository_impl.dart
│     ├─ question_repository_impl.dart
│     ├─ history_repository_impl.dart
│     └─ repositories_providers.dart   # 将 datasource 转换为 domain repository 的实现 Provider（例如 WordRepository
│
├─ domain/
│  ├─ entities/
│  │  ├─ word.dart
│  │  ├─ question.dart
│  │  └─ error_analysis.dart         # ErrorCause enum, ErrorAnalysisResult
│  ├─ repositories/                  # 抽象接口 (contract)
│  │  ├─ i_word_repository.dart
│  │  ├─ i_question_repository.dart
│  │  └─ i_history_repository.dart
│  ├─ services/                      # Domain-level service 抽象
│  │  ├─ i_error_analysis_service.dart  # 错因分析服务的接口（实现放在 data 层）
│  │  └─ i_scheduler_service.dart
│  └─ usecases/
│     ├─ analyze_spelling_error.dart
│     ├─ get_next_batch.dart
│     └─ usecases_providers.dart     # 把 repository 注入并构建 UseCase 对象（例如 AnalyzeSpellingErrorUseCase）
│
└─ features/
   ├─ common_widgets/
   │  └─ ...
   ├─ learn/
   │  ├─ state/
   │  │  └─ learn_state.dart   # 记录当前题目列表、索引、分数、加载状态、错误信息等
   │  ├─ ui/
   │  │  └─ learn_screen.dart
   │  └─ viewmodel/
   │     └─ learn_viewmodel.dart # 负责拉题、提交答案、调用错因分析、更新 Schedule；通过 StateNotifier 提供给 UI 使用。
   ├─ home/
   │  ├─ ui/home_screen.dart
   │  └─ viewmodel/home_viewmodel.dart
   └─ question_list/
      ├─ ui/question_list_screen.dart
      └─ viewmodel/question_list_viewmodel.dart
