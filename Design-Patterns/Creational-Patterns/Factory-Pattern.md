## The Concept 

Define an interface for creating an object, but let subclasses decide which class to instantiate. It lets a class defer instantiation to subclasses.

## Brutal Clarity 

You are writing a program that processes data, but you don't know ahead of time if the data will come as a CSV, a JSON file, or a Parquet file. If you use if/else statements everywhere (if type == 'csv': make_csv_parser()), your code becomes a mess of dependencies. Instead, you create a "Factory" that says, "I don't care how you create the parser, just give me a parser that follows the standard interface."

### When to use

- You don't know beforehand the exact types and dependencies of the objects your code should work with.

- You want to provide a library of products (like different database drivers) that users can extend.

### Pitfalls

- The code can become complicated since you need to introduce a lot of new subclasses (a subclass for every new product type) to implement the pattern.

## Examples

### Data Ingestion

You are writing a tool to ingest data into a data lake. You want to support JSON, and CSV.

```
class DataParser(ABC):
    @abstractmethod
    def parse(self, data):
        pass


class JSONParser(DataParser):
    def parse(self, data):
        return f"Parsing JSON data: {data}"


class CSVParser(DataParser):
    def parse(self, data):
        return f"Parsing CSV row: {data}"


class ParserFactory:
    @staticmethod
    def get_parser(file_type):
        if file_type == "json":
            return JSONParser()
        elif file_type == "csv":
            return CSVParser()
        else:
            raise ValueError("Unknown format")


file_types = ["json", "csv"]
data_content = "sample data"

for f_type in file_types:
    parser = ParserFactory.get_parser(f_type)
    print(parser.parse(data_content))

```