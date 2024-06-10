# gty

> A simple way to do GET http requests in flutter

### Installation

Add this to your package's pubspec.yaml file:

```yaml
gty:
  git:
    url: https://github.com/C4co/gty.git
```

## Using gty Mixin

The gty mixin is designed for Flutter's StatefulWidget to streamline HTTP GET requests and manage the loading, success, and error states. It provides a robust way to handle API interactions with built-in state management.

**Features**:

- **State Management**: Automatically handles isLoading, isSuccess, and isError states during API requests.
- **Response Handling**: Parses JSON responses and provides easy access to transform the response data.
- **Custom Headers**: Allows for custom headers.
- **Error Handling**: Catches and manages errors gracefully.

**States**:

- `isLoading`: Indicates that a request is in progress.
- `isSuccess`: Indicates that the request completed successfully.
- `isError`: Indicates that an error occurred during the request.
- `response`: Holds the full HTTP response from the request.
- `error`: Contains the error information if the request fails.
- `data`: Provides access to the parsed JSON response data.
- `viewData`: Provides a customized view of the response data, ex: adapting the data to a specific model.

**fetchData({url, headers, onSuccess})**:

- `url (required)`: The URL to which the HTTP GET request is sent.
- `headers (optional)`: Additional headers to include in the request.
- `onSuccess (optional)`: A callback function that is executed if the request is successful. This function receives the parsed JSON data as an argument and can be used to process the data.

**A complete example**:

```dart
import 'package:flutter/material.dart';
import 'package:gty/gty.dart';
import '../models/user.dart';

class ExampleOne extends StatefulWidget {
  const ExampleOne({super.key});

  @override
  State<ExampleOne> createState() => _ExampleOneState();
}

class _ExampleOneState extends State<ExampleOne> with gty {
  @override
  void initState() {
    super.initState();

    fetchData(
      url: "http://localhost:8080/users/1",
      onSuccess: (data) => adapt<User>(data, User.fromJson),
    );
  }

  @override
  Widget build(BuildContext context) {
    if (isLoading) {
      return const Center(
        child: Text("Loading..."),
      );
    }

    if (isError) {
      return const Center(
        child: Text("Error"),
      );
    }

    return Center(
      child: Text((viewData as User).name),
    );
  }
}
```

## Using Gty Widget

The `Gty` widget is a custom `StatefulWidget` designed to simplify making HTTP GET requests and handling different states such as loading, success, and error. It utilizes the gty mixin to manage the request lifecycle and provide a flexible way to render UI based on the request's state.

| Parameter    | Type                                                         | Description                                                                            |
| ------------ | ------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| `url`        | `String?`                                                    | The URL to send the HTTP GET request to.                                               |
| `child`      | `Widget Function(BuildContext context, dynamic data)`        | A function that returns a widget, given the BuildContext and fetched data.             |
| `error`      | `Widget Function(BuildContext context, GtyException error)?` | A function that returns a widget, given the BuildContext and an error.                 |
| `loading`    | `Widget`                                                     | A widget to display while the data is being fetched. Defaults to `Text('Loading...')`. |
| `onSuccess`  | `Function(dynamic)?`                                         | A callback function that is called when the request is successful.                     |
| `httpClient` | `http.Client?`                                               | An optional custom HTTP client.                                                        |

**Usage**:

```dart
import 'package:flutter/material.dart';
import 'package:gty/gty.dart';
import '../models/user.dart';

class ExampleWidget extends StatelessWidget {
  const ExampleWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Gty(
      url: "http://localhost:8080/users/1",
      loading: const Text("Loading..."),
      error: (context, error) => Center(
        child: Text(error.message),
      ),
      onSuccess: (data) => adapt<User>(data, User.fromJson),
      child: (context, data) {
        return Text(
          "${(data as User).name} - From example widget",
        );
      },
    );
  }
}
```

---

Carlos Costa @ 2024

