# Flutter Core Library

Reusable MVVM base classes and utilities. Single import:

```dart
import 'package:core/core_exports.dart';
```

## Setup

```yaml
dependencies:
  core:
    path: ../path/to/flutter_core
  provider: ^6.0.0
  dio: ^5.0.0
  uuid: ^4.0.0
```

## Implementation Order

1. **App config** — extend `CoreBaseAppInjection`, assign to `CoreAppImplementation.baseStructures`
2. **DI** — implement `BaseDependencyInjection`, register services at startup
3. **Models** — extend `CoreBaseModel` (or `CoreUserRelatedModel`); use `Result<T>` and `ResultState`
4. **Data layer** — pick one:
   - REST → `CoreDioService`
   - Firestore → `CoreFirestoreRepository<T>`
   - SQLite → `CoreSqfLiteRepository<T>`
   - JSON assets → `CoreJsonFileRepository<T>`
5. **Auth** (optional) — extend `CoreAuthService<T>` or use `CoreFirebaseAuthService`
6. **Screens** — `BaseViewModel` + `BaseWidget` + `ResultStateView`
7. **Errors** — `CoreException` subclasses; `catchResult()` async, `tryCatch()` sync
8. **Logging** — `CoreLogger.forClass(MyClass)`; ViewModels already expose `logger`

## Patterns

### MVVM screen

```dart
class MyViewModel extends BaseViewModel {
  MyViewModel({required super.context});

  Future<void> load() async {
    setState(ResultState.loading);
    final result = await catchResult(() => repository.getList());
    setState(result.isSuccess ? ResultState.success : ResultState.error);
  }
}

BaseWidget<MyViewModel>(
  viewModel: MyViewModel(context: context),
  onModelReady: (viewModel) => viewModel.load(),
  builder: (context, viewModel, child) => ResultStateView(
    state: viewModel.state,
    onSuccess: MyContent(),
    onLoading: CircularProgressIndicator(),
    onError: Text('Retry'),
    onEmpty: Text('No data'),
  ),
);
```

### HTTP

```dart
final service = CoreDioService()
  ..setDefault(
    baseUrl: 'https://api.example.com',
    headers: {'Authorization': 'Bearer $token'},
    timeout: const Duration(seconds: 30),
  );
await service.get<List<User>>('/users');
await service.post('/users', body: userModel); // body must implement Mapper
```

Errors from Dio are mapped automatically via `NetworkErrorHandler` (`BadRequestException`, `UnauthorizedException`, etc.).

### Repository (JSON example)

```dart
class ItemRepository extends CoreJsonFileRepository<Item> {
  ItemRepository()
      : super(
          assetPath: 'assets/data/items.json',
          fromJson: Item.fromJson,
          toJson: (item) => item.toJson(),
        );
}
```

### UI helpers

- `coreDialog(context: context, child: widget)` — modal dialog
- `CoreToastService()` — `showErrorToast`, `showSuccessToast`, etc.
- Widgets: `CoreButton`, `CoreTextField`, `CoreText`, `CoreBottomSheet`, `ResultStateView`

## Quick Reference

| Area | Key types |
|------|-----------|
| Logging | `CoreLogger`, `ICoreLogger`, `FirebaseCrashlyticsLogger` |
| DI | `BaseDependencyInjection`, `CoreBaseAppInjection` |
| App config | `CoreAppImplementation.baseStructures` |
| Errors | `CoreException`, `NetworkErrorHandler`, `tryCatch`, `catchResult` |
| HTTP | `CoreBaseService`, `CoreDioService` |
| Models | `Result<T>`, `ResultState`, `CoreBaseModel`, `Mapper` |
| Screens | `BaseViewModel`, `BaseWidget`, `ResultStateView` |
| Repositories | `CoreRepository<T>`, Firestore / SQLite / JSON implementations |
| Auth | `CoreAuthService<T>`, `CoreFirebaseAuthService` |
| Storage | `CoreSharedPref`, `CoreFileStorage` |
| IAP | `CoreIapService`, `IapProduct`, `IapTransaction` |

Use the library source for full method signatures and optional deps (Firebase, sqflite, file_picker, etc.).
