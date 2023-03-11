[[Elixir]]

# config.exs
This file is typically used to store configuration values that are set once and do not change during the lifetime of the application. This values are common to the whole application and are cached for the lifetime of the application?
This file is only read at compile time.

# runtime.exs
For runtime configuration, you can use the `config/runtime.ex` file. It is executed right before applications start in both Mix and releases.
This file is tipically used to store configuration values related to feature flags or other settings that can be toggled on or off during runtime?
It is also woth noting that the `runtime.exs` file is loaded after the `config.exs` file. This also means that this file is read everytime the project is started and not only when it's compiled like `config.exs`.

# dev.exs
This file is used to configure the application for developing purposes. It offers features such as the `:debug` logging which allows for better and more verbose logging messages or configure the application to reload modules automatically when changes are made.

# prod.exs
This file on the other hand is used to configure the application in the production environment. It includes settings that optimize the application for performance and reliabiliity. For example, you might set the logging level to `:info` or configure the server to use a different database or server. 
