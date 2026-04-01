# AI Coding Rules - Flutter Clean Architecture (Feature Based Pattern)

> **Purpose:** This document provides AI coding assistants with strict rules and patterns for generating Flutter code following Clean Architecture with Riverpod state management.

---

## 🤖 AI AGENT FIRST STEP — ssl_cli Check (MANDATORY)

> ⚠️ **Before writing ANY code or creating ANY file or folder manually, the AI agent MUST follow this checklist.**

### Step 1 — Check if ssl_cli is Installed

Run this command first:

```bash
ssl_cli help --all
```

- ✅ **If output is shown** → ssl_cli is installed. Proceed to Step 2.
- ❌ **If command not found** → Install it first:

```bash
dart pub global activate ssl_cli
```

Then verify PATH is set:
- **macOS/Linux:** `export PATH="$PATH":"$HOME/.pub-cache/bin"` → add to `~/.zshrc` or `~/.bashrc`
- **Windows:** Add Dart pub cache to System Environment Variables

After install, re-run `ssl_cli help --all` to confirm.

---

### Step 2 — Use ssl_cli for ALL Scaffolding (NEVER manually create structure)

> 🚫 **AI agents MUST NOT manually create folders/files for project or module scaffolding.**
> ✅ **ALWAYS use ssl_cli commands. This saves tokens and ensures consistent structure.**

#### Creating a New Project

```bash
ssl_cli create <project_name>
```
- When prompted for pattern → **select pattern `4`** (Clean Architecture)
- When prompted for state management → **select `Riverpod`**

#### Adding a New Feature Module

```bash
ssl_cli module <module_name>
```
- When prompted for pattern → **select Clean Architecture pattern `3`**
- When prompted for state management → **select `Riverpod`**

#### After Adding Assets (Images / SVGs)

> ⚠️ **Whenever any image or SVG file is added to the assets folder, the AI agent MUST run this command immediately:**

```bash
ssl_cli generate k_assets.dart
```

**Rules:**
- ✅ ALWAYS run after adding any `.png`, `.jpg`, `.jpeg`, `.svg` file
- ✅ Reference assets via generated enum only (e.g. `ImageNamePng.myImage`, `SvgName.myIcon`)
- ❌ NEVER hardcode asset paths as raw strings

#### Build & Release

```bash
ssl_cli build apk --flavorType       # --DEV / --LIVE / --LOCAL / --STAGE
ssl_cli build apk --flavorType --t   # Build + auto-share to Telegram
```

---

### Step 3 — After ssl_cli Scaffolding, Fill in the Logic

Once ssl_cli generates the structure, the AI agent fills in:
- Entity fields
- Model `fromJson` / `toJson`
- UseCase business logic
- Repository implementation
- Provider state & actions
- UI page & widgets

> Only write code **inside** the generated files. Never create new folders manually.

---

### ssl_cli Agent Decision Flow

```
AI receives a task
       ↓
Run: ssl_cli help --all
       ↓
Found? ──No──→ Run: dart pub global activate ssl_cli → verify → continue
       │
      Yes
       ↓
Is it a new project? ──Yes──→ ssl_cli create <project_name> (pick pattern 4 + Riverpod)
       │
      No
       ↓
Is it a new feature? ──Yes──→ ssl_cli module <module_name> (pick pattern 3 + Riverpod)
       │
      No
       ↓
Added new assets? ──Yes──→ ssl_cli generate k_assets.dart
       │
      No
       ↓
Fill in logic inside generated files following rules below
```

---

## 🎯 Core Architecture Pattern

This project follows **Clean Architecture** with **Riverpod** state management. All code generation MUST follow this three-layer structure:

```
Domain Layer (Business Logic) → Data Layer (Data Management) → Presentation Layer (UI)
```

**Dependency Rule:** Dependencies ONLY point inward. Domain has NO dependencies on outer layers.

---

## 📁 Mandatory Project Structure

### Feature Module Structure (STRICT)

When creating ANY new feature, you MUST create this exact folder structure:

```
lib/features/{feature_name}/
├── data/
│   ├── datasources/
│   │   ├── {feature}_remote_datasource.dart
│   │   └── {feature}_local_datasource.dart
│   ├── models/
│   │   └── {model_name}_model.dart
│   └── repositories/
│       └── {feature}_repository_impl.dart
├── domain/
│   ├── entities/
│   │   └── {entity_name}_entity.dart
│   ├── repositories/
│   │   └── {feature}_repository.dart
│   └── usecases/
│       └── {action}_usecase.dart
└── presentation/
    ├── pages/
    │   └── {page_name}_page.dart
    ├── providers/
    │   ├── {feature}_provider.dart
    │   └── state/
    │       └── {feature}_state.dart
    └── widgets/
        └── {widget_name}.dart
```

### Core Structure (Shared Infrastructure)

```
lib/core/
├── constants/          # API URLs, app constants
├── di/                # Dependency injection (GetIt)
├── entities/          # Base entities
├── error/             # Exceptions and failures
├── models/            # Global models
├── network/           # API client, network info
├── presentation/
│   ├── widgets/       # Global reusable widgets
│   └── mixins/        # Shared presentation logic
├── routes/            # Navigation
├── theme/             # Theme, colors
├── usecases/          # Base UseCase interface
└── utils/             # Helpers, extensions
```

---

## 🔧 Code Generation Rules

### 1. Domain Layer Rules

#### Entity Template

```dart
// lib/features/{feature}/domain/entities/{entity_name}_entity.dart
import 'package:equatable/equatable.dart';

class {EntityName}Entity extends Equatable {
  final String id;
  final String name;
  // Add fields here
  
  const {EntityName}Entity({
    required this.id,
    required this.name,
  });
  
  @override
  List<Object?> get props => [id, name];
}
```

**Rules:**
- ✅ MUST extend `Equatable`
- ✅ MUST be immutable (`const` constructor, `final` fields)
- ✅ NO Flutter imports
- ✅ NO external package dependencies (except `equatable`, `dartz`)

#### Repository Contract Template

```dart
// lib/features/{feature}/domain/repositories/{feature}_repository.dart
import 'package:dartz/dartz.dart';
import '../../../../core/error/failures.dart';
import '../entities/{entity_name}_entity.dart';

abstract class {Feature}Repository {
  Future<Either<Failure, {Entity}Entity>> get{Entity}(String id);
  Future<Either<Failure, List<{Entity}Entity>>> get{Entity}List();
  Future<Either<Failure, void>> create{Entity}({Entity}Entity entity);
  Future<Either<Failure, void>> update{Entity}({Entity}Entity entity);
  Future<Either<Failure, void>> delete{Entity}(String id);
}
```

**Rules:**
- ✅ MUST be abstract class
- ✅ MUST return `Either<Failure, T>`
- ✅ MUST use entities, NOT models

#### UseCase Template

```dart
// lib/features/{feature}/domain/usecases/{action}_usecase.dart
import 'package:dartz/dartz.dart';
import 'package:equatable/equatable.dart';
import '../../../../core/error/failures.dart';
import '../../../../core/usecases/usecase.dart';
import '../entities/{entity_name}_entity.dart';
import '../repositories/{feature}_repository.dart';

class {Action}UseCase implements UseCase<{Return}Entity, {Action}Params> {
  final {Feature}Repository repository;
  
  {Action}UseCase({required this.repository});
  
  @override
  Future<Either<Failure, {Return}Entity>> call({Action}Params params) async {
    return await repository.{action}(params);
  }
}

class {Action}Params extends Equatable {
  final String id;
  // Add parameters here
  
  const {Action}Params({
    required this.id,
  });
  
  @override
  List<Object?> get props => [id];
}
```

**Rules:**
- ✅ MUST implement `UseCase<ReturnType, ParamsType>`
- ✅ Params MUST extend `Equatable`
- ✅ One use case = one business action
- ✅ MUST inject repository through constructor

### 2. Data Layer Rules

#### Model Template

```dart
// lib/features/{feature}/data/models/{model_name}_model.dart
import '../../domain/entities/{entity_name}_entity.dart';

class {Model}Model extends {Entity}Entity {
  const {Model}Model({
    required super.id,
    required super.name,
  });
  
  factory {Model}Model.fromJson(Map<String, dynamic> json) {
    return {Model}Model(
      id: json['id'] as String,
      name: json['name'] as String,
    );
  }
  
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
    };
  }
}
```

**Rules:**
- ✅ MUST extend corresponding entity
- ✅ MUST have `fromJson` factory
- ✅ MUST have `toJson` method

#### Remote Data Source Template

```dart
// lib/features/{feature}/data/datasources/{feature}_remote_datasource.dart
import 'package:dio/dio.dart';
import '../../../../core/constants/api_urls.dart';
import '../../../../core/network/api_client.dart';
import '../models/{model_name}_model.dart';

abstract class {Feature}RemoteDataSource {
  Future<{Model}Model> get{Entity}(String id);
  Future<List<{Model}Model>> get{Entity}List();
}

class {Feature}RemoteDataSourceImpl implements {Feature}RemoteDataSource {
  final ApiClient _apiClient;
  
  {Feature}RemoteDataSourceImpl({required ApiClient apiClient})
      : _apiClient = apiClient;
  
  @override
  Future<{Model}Model> get{Entity}(String id) async {
    try {
      final response = await _apiClient.request(
        endpoint: ApiUrl.{endpoint}.url,
        method: HttpMethod.get,
        queryParameters: {'id': id},
      );
      return {Model}Model.fromJson(response);
    } catch (e) {
      rethrow;
    }
  }
  
  @override
  Future<List<{Model}Model>> get{Entity}List() async {
    try {
      final response = await _apiClient.request(
        endpoint: ApiUrl.{endpoint}.url,
        method: HttpMethod.get,
      );
      return (response['data'] as List)
          .map((json) => {Model}Model.fromJson(json))
          .toList();
    } catch (e) {
      rethrow;
    }
  }
}
```

**Rules:**
- ✅ MUST have abstract class and implementation
- ✅ MUST inject `ApiClient` (NOT Dio directly)
- ✅ MUST use `ApiUrl` enum for endpoints
- ✅ MUST use handleException

#### Repository Implementation Template

```dart
// lib/features/{feature}/data/repositories/{feature}_repository_impl.dart
import 'package:dartz/dartz.dart';
import 'dart:io';
import '../../../../core/error/exceptions.dart';
import '../../../../core/error/failures.dart';
import '../../domain/entities/{entity_name}_entity.dart';
import '../../domain/repositories/{feature}_repository.dart';
import '../datasources/{feature}_remote_datasource.dart';
import '../datasources/{feature}_local_datasource.dart';
import '../models/{model_name}_model.dart';

class {Feature}RepositoryImpl implements {Feature}Repository {
  final {Feature}RemoteDataSource remoteDataSource;
  final {Feature}LocalDataSource localDataSource;
  
  {Feature}RepositoryImpl({
    required this.remoteDataSource,
    required this.localDataSource,
  });
  
  @override
  Future<Either<Failure, {Entity}Entity>> get{Entity}(String id) async {
    return handleException(
      () async {
        final result = await remoteDataSource.get{Entity}(id);
        return result;
      },
    );
  }
}
```

**Rules:**
- ✅ MUST implement domain repository contract
- ✅ MUST inject data sources
- ✅ MUST convert exceptions to failures
- ✅ MUST return `Either<Failure, Entity>`
- ✅ Handle `ServerException`, `SocketException`, and generic exceptions

### 3. Presentation Layer Rules

#### Provider Template (Riverpod Code Generation)

```dart
// lib/features/{feature}/presentation/providers/{feature}_provider.dart
import '../../../../core/di/service_locator.dart';
import '../../domain/usecases/{action}_usecase.dart';
import 'state/{feature}_state.dart';

import 'package:flutter_riverpod/flutter_riverpod.dart';

class {Feature}Notifier extends Notifier<{Feature}State> {
  @override
  {Feature}State build() => const {Feature}State();
  
  Future<void> {action}({required String param}) async {
    final useCase = await sl<{Action}UseCase>();
    final result = await useCase({Action}Params(param: param));
   
    result.fold(
      (failure) => state = state.copyWith(failure: failure),
      (entity){
        // state = state.copyWith(entity: entity);
      },
    );
  }
}
```

**Rules:**
- ✅ MUST use `sl<UseCase>()` for dependency injection
- ✅ MUST update state based on `Either` result
- ✅ Call use cases, NOT repositories directly

#### Page Template

```dart
// lib/features/{feature}/presentation/pages/{page_name}_page.dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:flutter_screenutil/flutter_screenutil.dart';
import '../../../../core/presentation/widgets/global_text.dart';
import '../../../../core/presentation/widgets/global_button.dart';
import '../../../../core/presentation/widgets/global_loader.dart';
import '../providers/{feature}_provider.dart';

class {Page}Page extends ConsumerWidget {
  const {Page}Page({super.key});
  
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final state = ref.watch({feature}NotifierProvider);
    
    return Scaffold(
      appBar: AppBar(title: const GlobalText(str: '{Page}')),
      body: state.when(
        initial: () => const Center(child: GlobalText(str: 'Initial')),
        loading: () => const Center(child: GlobalLoader()),
        success: (entity) => const Center(child: GlobalText(str: 'Success')),
        error: (message) => const Center(child: GlobalText(str: 'Error')),
      ),
    );
  }
}
```

**Rules:**
- ✅ MUST use `StatelessWidget` if need Stateful then use `ConsumerWidget` if you need statefulWidget then use `ConsumerStatefulWidget`
- ✅ MUST use `ref.watch()` for state
- ✅ MUST use `ref.read().notifier` for actions
- ✅ MUST use global widgets (GlobalText, GlobalButton, etc.)
- ✅ MUST use ScreenUtil (.w, .h, .sp, .r)
- ✅ Always create component-based widget and place in `lib/features/{feature}/presentation/widgets`
- ✅ When creating component-based widget always try to use `StatelessWidget`

---

## 🎨 UI Component Rules

### Global Widgets (MANDATORY)

**NEVER use base Flutter widgets. ALWAYS use global widgets:**

| ❌ DON'T USE | ✅ USE INSTEAD |
|-------------|---------------|
| `Text()` | `GlobalText()` |
| `ElevatedButton()` | `GlobalButton()` |
| `TextFormField()` | `GlobalTextFormField()` |
| `DropdownButton()` | `GlobalDropdown()` |
| `Image.asset()` | `GlobalImageLoader()` |
| `CircularProgressIndicator()` | `GlobalLoader()` |
| `AppBar()` | `GlobalAppBar()` |
| `snackbar` | `ViewUtil.snackbar(context, message)` |
| `showDialog` | `ViewUtil.showDialog(context, message)` |
| `showBottomSheet` | `ViewUtil.showBottomSheet(context, message)` |
| `showSnackBar` | `ViewUtil.showSnackBar(context, message)` |

### GlobalText Usage
> For `GlobalText` don't use `.sp` as it is handled internally

```dart
GlobalText(
  str: 'Hello World',
  fontSize: 16,
)
```

### GlobalButton Usage

```dart
GlobalButton(
  btnText: 'Submit',
  onPressed: (){},
)
```

### GlobalTextFormField Usage

```dart
GlobalTextFormField(
  hintText: 'Enter name',
  labelText: 'Name',
  validator: (value) => value?.isEmpty ?? true ? 'Required' : null,
)
```

### Responsive Sizing (MANDATORY)

```dart
// ❌ DON'T
Container(width: 200, height: 100)
Text('Hello', style: TextStyle(fontSize: 16))

// ✅ DO
Container(width: 200.w, height: 100.h)
GlobalText(str: 'Hello', fontSize: 16.sp)
```

**ScreenUtil Extensions:**
- `.w` - Responsive width
- `.h` - Responsive height
- `.sp` - Responsive font size
- `.r` - Responsive radius/padding

---

## 🔗 Dependency Injection Rules

### Registration Pattern (service_locator.dart)

When adding a new feature, ALWAYS register dependencies in this order:

```dart
// 1. Data Sources
sl.registerLazySingleton<{Feature}RemoteDataSource>(
  () => {Feature}RemoteDataSourceImpl(apiClient: sl()),
);

sl.registerLazySingleton<{Feature}LocalDataSource>(
  () => {Feature}LocalDataSourceImpl(prefHelper: sl()),
);

// 2. Repository
sl.registerLazySingleton<{Feature}Repository>(
  () => {Feature}RepositoryImpl(
    remoteDataSource: sl(),
    localDataSource: sl(),
  ),
);

// 3. Use Cases (Factory)
sl.registerFactory(() => {Action}UseCase(repository: sl()));
```

**Rules:**
- ✅ Data sources & repositories: `registerLazySingleton`
- ✅ Use cases: `registerFactory`
- ✅ MUST register in `initDependencies()` function
- ✅ MUST call `sl()` to resolve dependencies

---

## 📝 Naming Conventions (STRICT)

### Files

| Type | Pattern | Example |
|------|---------|---------|
| Entity | `{name}_entity.dart` | `user_entity.dart` |
| Model | `{name}_model.dart` | `user_model.dart` |
| UseCase | `{action}_usecase.dart` | `get_user_usecase.dart` |
| Repository | `{feature}_repository.dart` | `auth_repository.dart` |
| Repository Impl | `{feature}_repository_impl.dart` | `auth_repository_impl.dart` |
| Data Source | `{feature}_{type}_datasource.dart` | `auth_remote_datasource.dart` |
| Provider | `{feature}_provider.dart` | `login_provider.dart` |
| State | `{feature}_state.dart` | `login_state.dart` |
| Page | `{name}_page.dart` | `login_page.dart` |

### Classes

| Type | Pattern | Example |
|------|---------|---------|
| Entity | `{Name}Entity` | `UserEntity` |
| Model | `{Name}Model` | `UserModel` |
| UseCase | `{Action}UseCase` | `GetUserUseCase` |
| Params | `{Action}Params` | `GetUserParams` |
| Repository | `{Feature}Repository` | `AuthRepository` |
| Repository Impl | `{Feature}RepositoryImpl` | `AuthRepositoryImpl` |
| Data Source | `{Feature}{Type}DataSource` | `AuthRemoteDataSource` |
| Provider | `{Feature}Provider` | `LoginProvider` |
| State | `{Feature}State` | `LoginState` |
| Page | `{Name}Page` | `LoginPage` |

---

## ⚠️ Error Handling Pattern (MANDATORY)

### Exception Hierarchy

```dart
// core/error/exceptions.dart
class ServerException implements Exception {
  final String message;
  ServerException({required this.message});
}
```

### Failure Hierarchy

```dart
// core/error/failures.dart
import 'package:equatable/equatable.dart';

abstract class Failure extends Equatable {
  final String message;
  const Failure({required this.message});
  
  @override
  List<Object> get props => [message];
}

class NetworkFailure extends Failure {
  const NetworkFailure({required super.message});
}
```

### Error Flow

1. **Data Source:** Throw exceptions
2. **Repository:** Catch exceptions → Return `Left(Failure)`
3. **Use Case:** Pass through `Either<Failure, Data>`
4. **Provider:** Handle with `fold()` → Update state

---

## ✅ Feature Creation Checklist

When creating a new feature, follow this order:

**0. CLI Setup**
- [ ] Run `ssl_cli help --all` to verify ssl_cli is installed
- [ ] If not installed → run `dart pub global activate ssl_cli`
- [ ] Run `ssl_cli module <module_name>` → select pattern 3 + Riverpod

**1. Domain Layer**
- [ ] Create entity in `domain/entities/`
- [ ] Create repository contract in `domain/repositories/`
- [ ] Create use cases in `domain/usecases/`

**2. Data Layer**
- [ ] Create model in `data/models/` (extends entity)
- [ ] Create remote data source in `data/datasources/`
- [ ] Create local data source in `data/datasources/` (if needed)
- [ ] Create repository implementation in `data/repositories/`

**3. Dependency Injection**
- [ ] Register data sources in `service_locator.dart`
- [ ] Register repository in `service_locator.dart`
- [ ] Register use cases in `service_locator.dart`

**4. Presentation Layer**
- [ ] Create state in `presentation/providers/state/`
- [ ] Create provider in `presentation/providers/`
- [ ] Create page in `presentation/pages/`
- [ ] Create widgets in `presentation/widgets/` (if needed)

**5. Post Steps**
- [ ] Run `ssl_cli generate k_assets.dart` (if new assets added)

---

## 🎯 Quick Reference: Common Patterns

### API Call Pattern

```dart
// 1. Define endpoint in core/constants/api_urls.dart
enum ApiUrl {
  getUsers,
}

extension ApiUrlExtension on ApiUrl {
  String get url {
    switch (this) {
      case ApiUrl.getUsers:
        return '/users';
    }
  }
}

// 2. Use in data source
final response = await _apiClient.request(
  endpoint: ApiUrl.getUsers.url,
  method: HttpMethod.get,
);
```

### Navigation Pattern

```dart
// Use centralized navigation
Navigation.push(context, appRoutes: AppRoutes.login);
Navigation.pop(context);
```

### Theme/Color Pattern

```dart
// Define in core/theme/app_colors.dart
enum AppColors {
  scaffold(Color.fromARGB(255, 222, 242, 240)),
  primary(Color(0xFF28294D)),
  primaryLight(Color(0xFF42A5F5)),
  
  final Color color;
  const AppColors(this.color);
}

// Use in widgets
Container(color: AppColors.primary.color)
```

---

## 🚫 Common Mistakes to Avoid

1. ❌ NOT checking ssl_cli before scaffolding (always check first)
2. ❌ Manually creating folders/files instead of using ssl_cli
3. ❌ Using Flutter widgets directly instead of global widgets
4. ❌ Importing repositories in presentation layer (use use cases)
5. ❌ Returning models from use cases (use entities)
6. ❌ Hard-coded sizes without ScreenUtil
7. ❌ Forgetting to register dependencies in service_locator
8. ❌ Not handling all state cases in UI
9. ❌ Using `registerLazySingleton` for use cases (use `registerFactory`)
10. ❌ Throwing failures instead of exceptions in data layer
11. ❌ Not running `ssl_cli generate k_assets.dart` after adding new images or SVGs

---

## 🤖 AI Assistant Summary Instructions

When generating code for this project:

1. **FIRST** run `ssl_cli help --all` — if not found, install via `dart pub global activate ssl_cli`
2. **ALWAYS** use `ssl_cli create` for new projects and `ssl_cli module` for new features
3. **NEVER** manually create folder structure — let ssl_cli do it
4. **ALWAYS** follow the exact folder structure defined above
5. **ALWAYS** use the provided templates to fill in logic
6. **ALWAYS** use global widgets instead of base Flutter widgets
7. **ALWAYS** use ScreenUtil for sizing
8. **ALWAYS** register dependencies in service_locator.dart
9. **ALWAYS** handle errors with `Either<Failure, Data>`
10. **ALWAYS** use Riverpod for state management
11. **NEVER** skip layers (always domain → data → presentation)
12. **NEVER** use repositories directly in presentation (use use cases)

**This is a strict, opinionated architecture. Follow it exactly.**
