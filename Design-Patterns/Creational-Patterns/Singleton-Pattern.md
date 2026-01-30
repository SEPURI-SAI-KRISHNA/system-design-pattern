## The Concept 
The Singleton pattern ensures that a class has only one instance and provides a global point of access to that instance.

## Brutal Clarity 
Sometimes, you need exactly one object to coordinate actions across the system. If you have ten different parts of your code trying to write to the same log file, or thirty threads trying to access a database, you don't want thirty different connection managers. You want one manager that handles all requests.


## When to use

- Logging drivers

- Database connections

- Caching mechanisms

- Thread pools.

## Pitfalls

#### Global State
Singletons act like global variables. If you change the state in one part of the code, it changes everywhere. This can make debugging hard.

#### Concurrency
In multi-threaded environments (like Java or C++), you must ensure your Singleton creation is thread-safe (using locks/mutexes) so two threads don't create two instances simultaneously.


## Examples

### Database Connection Pool

Imagine you are building an API. Every time a user hits an endpoint, you don't want to establish a brand-new connection to PostgreSQL, authenticate, and handshake. That is slow and resource-heavy. You want one pool that stays

```
import sqlite3


class DatabaseConnection:
    _instance = None  # Helps hold a single instance

    def __new__(cls):
        if cls._instance is None:
            print("Creating new database connection")
            cls._instance = super(DatabaseConnection, cls).__new__(cls)
            cls._instance.connection = sqlite3.connect(':memory:')
        return cls._instance


db1 = DatabaseConnection()  # Prints: Creating new database connection...
db2 = DatabaseConnection()  # Does NOT print. Returns existing instance.

print(db1 is db2)  # Output: True (They are the exact same object in memory)

```


### Configuration Manager
In a data pipeline, you have config.yaml containing AWS keys, file paths, and retry limits. You don't want to read and parse this file every time a different transformation function runs. You read it once, load it into a Singleton, and every function reads from that single memory space.

```
class AppConfig:
    __instance = None  # Helps hold a single instance

    @staticmethod
    def get_instance():
        if AppConfig.__instance is None:
            AppConfig()
        return AppConfig.__instance

    def __init__(self):
        if AppConfig.__instance is not None:
            raise Exception("This class is a singleton! Use get_instance().")
        else:
            self.settings = {"retry_count": 3, "spark_master": "local[*]"}
            AppConfig.__instance = self


config1 = AppConfig.get_instance()
print(config1.settings["spark_master"])  # Output: local[*]

config2 = AppConfig()  # This would raise an Exception

```