In Python, a **Protocol** defines a set of methods and attributes that a class must provide to be considered compatible with that protocol.

Protocols are used for **structural subtyping (static duck typing)**. Unlike inheritance, a class does not need to explicitly inherit from a protocol.

Python provides `Protocol` through the `typing` module.

## **1. Basic example**

```
from typing import Protocolclass Animal(Protocol):    def speak(self) -> str:        ...class Dog:    def speak(self) -> str:        return "Woof!"class Cat:    def speak(self) -> str:        return "Meow!"def make_sound(animal: Animal):    print(animal.speak())make_sound(Dog())make_sound(Cat())
```

**What's happening?**

- `Animal` defines a contract.
- `Dog` and `Cat` implement the required method.
- Neither class explicitly inherits from `Animal`.
- Both are compatible because they implement `speak()`.

## **2. Protocol vs Abstract Base Class (ABC)**

| Feature              | Protocol          | ABC                                 |
| -------------------- | ----------------- | ----------------------------------- |
| Inheritance required | No                | Usually yes                         |
| Type checking        | Structural        | Nominal                             |
| Runtime enforcement  | No, by default    | Yes, for abstract methods           |
| Flexibility          | High              | More restrictive                    |
| Main purpose         | Define interfaces | Enforce inheritance-based contracts |

Example of ABC:

```
from abc import ABC, abstractmethodclass Animal(ABC):    @abstractmethod    def speak(self):        passclass Dog(Animal):    def speak(self):        return "Woof!"
```

## **3. Protocol with multiple methods**

```
from typing import Protocolclass Database(Protocol):    def connect(self) -> None:        ...    def execute(self, query: str) -> list:        ...class PostgreSQL:    def connect(self):        print("Connected to PostgreSQL")    def execute(self, query: str) -> list:        return ["data1", "data2"]
```

Any class implementing the required interface can be used as a `Database`.

## **4. Runtime-checkable Protocol**

By default, protocols are primarily intended for static type checking using tools like Pyright and mypy.

You can enable runtime structural checks using `@runtime_checkable`.

```
from typing import Protocol, runtime_checkable@runtime_checkableclass Logger(Protocol):    def log(self, message: str) -> None:        ...class ConsoleLogger:    def log(self, message: str) -> None:        print(message)logger = ConsoleLogger()print(isinstance(logger, Logger))  # True
```

Note: Runtime checks generally verify the presence of required attributes, not their full type signatures.

## **5. Where are Protocols useful?**

Protocols are particularly useful in:

- **FastAPI:** Defining service interfaces.
- **Dependency injection:** Decoupling implementations.
- **Clean architecture:** Separating business logic from infrastructure.
- **Testing:** Using mock implementations.
- **Large applications:** Making code easier to extend without tight coupling.

## **6. Real-world architecture example**

Imagine you have an application that supports multiple payment providers.

```
from typing import Protocolclass PaymentGateway(Protocol):    def pay(self, amount: float) -> bool:        ...class Razorpay:    def pay(self, amount: float) -> bool:        print(f"Razorpay payment: {amount}")        return Trueclass Stripe:    def pay(self, amount: float) -> bool:        print(f"Stripe payment: {amount}")        return Truedef checkout(gateway: PaymentGateway):    gateway.pay(1000)checkout(Razorpay())checkout(Stripe())
```

You can introduce a new payment provider without changing the `checkout()` function.

**Key takeaway:** Python Protocol is a way to define interfaces without forcing classes to inherit from a common parent. It makes code loosely coupled, flexible, and easier to test.

**Which deeper explanation would be most useful?**

&#x20;Protocol vs ABC in depth Protocols in FastAPI Advanced typing
