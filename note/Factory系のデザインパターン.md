## Factory
1つ、または複数のタイプのオブジェクトを作成するためのファクトリークラスを持つ。
インタフェースを実装しない。
```java
interface Product {
    void use();
}

class ConcreteProductA implements Product {
    @Override
    public void use() {
        System.out.println("Using Product A");
    }
}

class ConcreteProductB implements Product {
    @Override
    public void use() {
        System.out.println("Using Product B");
    }
}

class Factory {
    public static Product createProduct(String type) {
        switch (type) {
            case "A":
                return new ConcreteProductA();
            case "B":
                return new ConcreteProductB();
            default:
                throw new IllegalArgumentException("Unknown product type");
        }
    }
}
```
利用側：
```java
public class FactoryPatternDemo {
    public static void main(String[] args) {
        Product productA = Factory.createProduct("A");
        productA.use();
        
        Product productB = Factory.createProduct("B");
        productB.use();
    }
}
```

## Factory Method
1種類のオブジェクトのみを生成できるファクトリークラスを持つ。
Factoryと違って、1種類のオブジェクトのみなのでどんどんファクトリーが増える。
```java
interface Product {
    void use();
}

class ConcreteProductA implements Product {
    @Override
    public void use() {
        System.out.println("Using Product A");
    }
}

class ConcreteProductB implements Product {
    @Override
    public void use() {
        System.out.println("Using Product B");
    }
}

abstract class Creator {
    public abstract Product createProduct();
}

class ConcreteCreatorA extends Creator {
    @Override
    public Product createProduct() {
        return new ConcreteProductA();
    }
}

class ConcreteCreatorB extends Creator {
    @Override
    public Product createProduct() {
        return new ConcreteProductB();
    }
}
```
利用側：
```java
public class FactoryMethodPatternDemo {
    public static void main(String[] args) {
        Creator creatorA = new ConcreteCreatorA();
        Product productA = creatorA.createProduct();
        productA.use();
        
        Creator creatorB = new ConcreteCreatorB();
        Product productB = creatorB.createProduct();
        productB.use();
    }
}
```

## Abstract Factory
関連するオブジェクト群（ファミリー）を生成できるファクトリークラスを持つ。
Factoryと違って、インタフェースを利用している。
```java
interface ProductA {
    void use();
}

interface ProductB {
    void use();
}

class ConcreteProductA1 implements ProductA {
    @Override
    public void use() {
        System.out.println("Using Product A1");
    }
}

class ConcreteProductA2 implements ProductA {
    @Override
    public void use() {
        System.out.println("Using Product A2");
    }
}

class ConcreteProductB1 implements ProductB {
    @Override
    public void use() {
        System.out.println("Using Product B1");
    }
}

class ConcreteProductB2 implements ProductB {
    @Override
    public void use() {
        System.out.println("Using Product B2");
    }
}

interface AbstractFactory {
    ProductA createProductA();
    ProductB createProductB();
}

class ConcreteFactory1 implements AbstractFactory {
    @Override
    public ProductA createProductA() {
        return new ConcreteProductA1();
    }

    @Override
    public ProductB createProductB() {
        return new ConcreteProductB1();
    }
}

class ConcreteFactory2 implements AbstractFactory {
    @Override
    public ProductA createProductA() {
        return new ConcreteProductA2();
    }

    @Override
    public ProductB createProductB() {
        return new ConcreteProductB2();
    }
}
```
利用側：
```java
public class AbstractFactoryPatternDemo {
    public static void main(String[] args) {
        AbstractFactory factory1 = new ConcreteFactory1();
        ProductA productA1 = factory1.createProductA();
        ProductB productB1 = factory1.createProductB();
        productA1.use();
        productB1.use();
        
        AbstractFactory factory2 = new ConcreteFactory2();
        ProductA productA2 = factory2.createProductA();
        ProductB productB2 = factory2.createProductB();
        productA2.use();
        productB2.use();
    }
}
```

## 参考
[Difference between Factory and Abstract Factory | bitMountn (medium.com)](https://medium.com/bitmountn/factory-vs-factory-method-vs-abstract-factory-c3adaeb5ac9a)
