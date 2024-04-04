### 依赖注入（DI – Dependency Injection）
一种设计模式，Spring框架的核心概念之一，感觉就是多态？  
使用一个类时，如果在代码里直接指定，那么以后需要替换的时候所有的代码都需要修改。  
在构造函数或者setter里配置这种依赖关系的话，则可以简单的修改  
```java
    Player(){  
        this.weapon = new Sword();  
    }  
```
⬇
```java
    Player(Weapon weapon){  
        this.weapon = weapon;  
    }
```
或者
```java
    Player(){  
        public SetWeapon(Weapon weapon){
            this.weapon = weapon;
        }
    }
```
