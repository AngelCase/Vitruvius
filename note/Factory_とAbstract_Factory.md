参考サイト的に正しさは不明、ざっくりした理解。
## Factory
単一のインスタンスの生成を他のクラスに任せるパターン。
```python
class Factory:
    def create_product(self, product_type):
        if product_type == "A":
            return ConcreteProductA()
        elif product_type == "B":
            return ConcreteProductB()

class Product:
    pass

class ConcreteProductA(Product):
    pass

class ConcreteProductB(Product):
    pass

def main():
    factory = Factory()
    product_a = factory.create_product("A")
    product_b = factory.create_product("B")
```

## Abstract Factory
複数のオブジェクトを一度に生成したい場合、それを他のクラスに任せるパターン。
```python
class AbstractFactory:
    def create_product_a(self):
        pass

    def create_product_b(self):
        pass

class ConcreteFactoryA(AbstractFactory):
    def create_product_a(self):
        return ConcreteProductA()

    def create_product_b(self):
        return ConcreteProductB()

class ConcreteFactoryB(AbstractFactory):
    def create_product_a(self):
        return ConcreteProductC()

    def create_product_b(self):
        return ConcreteProductD()

class ProductA:
    pass

class ProductB:
    pass

class ConcreteProductA(ProductA):
    pass

class ConcreteProductB(ProductB):
    pass

class ConcreteProductC(ProductA):
    pass

class ConcreteProductD(ProductB):
    pass

def main():
    factory_a = ConcreteFactoryA()
    factory_b = ConcreteFactoryB()

    product_a = factory_a.create_product_a()
    product_b = factory_a.create_product_b()

    product_c = factory_b.create_product_a()
    product_d = factory_b.create_product_b()
```

## 参考
[【Python】Factory / Factory Method / Abstract Factory の違い - yiskw note (hatenablog.com)](https://yiskw713.hatenablog.com/entry/2023/01/22/151940#Abstract-Factory-%E3%81%A8%E3%81%AF)