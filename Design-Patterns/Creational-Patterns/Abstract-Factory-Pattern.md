## The Concept

The Abstract Factory is a super-factory which creates other factories. It provides an interface for creating families of related or dependent objects without specifying their concrete classes.

## Brutal Clarity

Imagine you are buying furniture.

- If you buy a Modern sofa, you also need a Modern chair and a Modern table to match.

- If you buy a Victorian sofa, you need a Victorian chair and table.

- You cannot mix a Modern Sofa with a Victorian table—it looks wrong (in code terms: "incompatible interfaces").

The Abstract Factory ensures that a client (your code) gets the right "family" of objects (Sofa + Chair + Table) without ever needing to know if it's "Modern" or "Victorian" at the code level.

### When to use

- When your code needs to work with various families of related products (e.g., AWS vs GCP vs Azure).

- You want to enforce that products from the same factory are used together (preventing the "Victorian chair with Modern table" problem).

### Pitfalls

- Rigidity: Adding a new type of product (e.g., adding Database to Storage and Compute) is painful. You have to update the CloudFactory interface and every single subclass (AWSFactory, GCPFactory, etc.).


## Example

### Cloud Infrastructure Provisioning
You are writing an Infrastructure-as-Code tool. You need to spin up a Storage unit and a Compute unit.

- On AWS, that means S3 and EC2.

- On Google Cloud, that means GCS and GCE. You cannot mix S3 with GCE in this specific module.

```
class Storage(ABC):
    @abstractmethod
    def store(self, data): pass


class Compute(ABC):
    @abstractmethod
    def process(self): pass


# (AWS)
class S3Storage(Storage):
    def store(self, data): print("Storing in S3 buckets")


class EC2Compute(Compute):
    def process(self): print("Processing on EC2 Instance")


# (GCP)
class GCSStorage(Storage):
    def store(self, data): print("Storing in Google Cloud Storage")


class GCECompute(Compute):
    def process(self): print("Processing on Google Compute Engine")


# Abstract Factory
class CloudFactory(ABC):
    @abstractmethod
    def create_storage(self) -> Storage: pass

    @abstractmethod
    def create_compute(self) -> Compute: pass


class AWSFactory(CloudFactory):
    def create_storage(self): return S3Storage()

    def create_compute(self): return EC2Compute()


class GCPFactory(CloudFactory):
    def create_storage(self): return GCSStorage()

    def create_compute(self): return GCECompute()


def run_job(factory: CloudFactory):
    storage = factory.create_storage()
    computer = factory.create_compute()

    computer.process()
    storage.store("Result Data")


run_job(AWSFactory())

```