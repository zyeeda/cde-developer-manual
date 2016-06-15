# 实体关系映射

###1. DomainEntity

DomainEntity 中定义了主键 id 属性，采用 FallbackUUIDHexGenerator 主键生成器。 FallbackUUIDHexGenerator 内部采用 UUID 策略生成主键，与普通生成器不同的是 FallbackUUIDHexGenerator 首先会对实体的主键进行检测，只在实体主键不存在的情况下才会为实体生成主键。也就是说 FallbackUUIDHexGenerator 允许使用者自行设置实体的主键。

如果不想自己设置实体的生成策略或编写主键生成器，可以让实体集成 DomainEntity ， 这样就可以不为实体的主键操心了。

###2. JPA 注解

平台基于 JPA 开发规范，因此支持所有 JPA 注解，我们推荐将注解放在方法上而非属性上。

**注意**，对于无 get 或 is 方法，又不是数据库字段的属性，应该加上 @Transient 注解。

###3. 详细说明

1、@Entity(name="EntityName")

必须，name 属性可选，对应数据库中一的个表。


2、@Table(name="", catalog="", schema="")

可选，通常和 @Entity 配合使用，只能标注在实体实体上，表示实体对应的数据库表的信息。

name:可选，表示表的名称。默认地，表名和实体名称一致，只有在不一致的情况下才需要指定表名。我们建议使用此属性为实体指定表名，并且分类为表名设置前缀。

catalog:可选，表示Catalog名称，默认为Catalog("")。

schema:可选，表示Schema名称，默认为Schema("")。

3、@id

必须

@id 定义了映射到数据库表的主键的属性，一个实体只能有一个属性被映射为主键。置于getXxxx()前。

继承 DomainEntity 的实体可以忽略此注解。



4、@GeneratedValue(strategy=GenerationType, generator="")

可选

strategy:表示主键生成策略，有AUTO，INDENTITY，SEQUENCE 和 TABLE 4种，分别表示让 ORM 框架自动选择，

根据数据库的 Identity 字段生成，根据数据库表的 Sequence 字段生成，以有根据一个额外的表生成主键，默认为 AUTO 。

generator:表示主键生成器的名称，这个属性通常和 ORM 框架相关，例如，Hibernate 可以指定 uuid 等主键生成方式。

示例:

```java
    @Id
    @GeneratedValues(strategy=StrategyType.SEQUENCE)
    public int getPk() {

       return pk;

    }
```

继承 DomainEntity 的实体可以忽略此注解。

5、@Basic(fetch=FetchType, optional=true)

可选

@Basic 表示一个简单的属性到数据库表的字段的映射，对于没有任何标注的 getXxxx() 方法，默认即为 @Basic 注解。

fetch: 表示该属性的读取策略，有 EAGER 和 LAZY 两种，分别表示主动抓取和延迟加载，默认为 EAGER.

optional:表示该属性是否允许为 null，默认为 true

示例:

```java
    @Basic(optional=false)
    public String getAddress() {

       return address;

    }
```

6、@Column

可选

@Column 描述了数据库表中该字段的详细定义，这对于根据 JPA 注解生成数据库表结构的工具非常有作用。

name:表示数据库表中该字段的名称，默认情形属性名称一致。

nullable:表示该字段是否允许为 null ，默认为 true。

unique:表示该字段是否是唯一标识，默认为 false 。

length:表示该字段的大小，仅对 String 类型的字段有效。

insertable:表示在 ORM 框架执行插入操作时，该字段是否应出现 INSETRT 语句中，默认为 true。

updateable:表示在 ORM 框架执行更新操作时，该字段是否应该出现在 UPDATE 语句中，默认为 true 。对于一经创建就不可以更改的字段，该属性非常有用，如对于 birthday 字段。

columnDefinition:表示该字段在数据库中的实际类型。通常 ORM 框架可以根据属性类型自动判断数据库中字段的类型，但是对于 Date 类型仍无法确定数据库中字段类型究竟是 DATE ，TIME 还是 TIMESTAMP 。此外，String 的默认映射类型为 VARCHAR ，如果要将 String 类型映射到特定数据库的 BLOB 或 TEXT 字段类型，该属性非常有用。

示例:

```java
    @Column(name="BIRTH", nullable="false", columnDefinition="DATE")
    public String getBithday() {

       return birthday;

    }
```


7、@Transient

可选

@Transient 表示该属性不需要与数据库表中的字段进行映射，ORM 框架将忽略该属性。

如果一个属性不需要与数据库表中的字段进行映射，就务必为其添加  @Transient 注解，否则 ORM 框架默认其注解为 @Basic 。

示例:

```java
    //根据birth计算出age属性
    @Transient
    public int getAge() {

       return getYear(new Date()) - getYear(birth);

    }
```

8、@ManyToOne(fetch=FetchType, cascade=CascadeType)

可选

@ManyToOne 表示一个多对一的映射，该注解标注的属性通常是数据库表的外键。

optional:是否允许该字段为 null，该属性应该根据数据库表的外键约束来确定，默认为 true 。

fetch:表示抓取策略，默认为 FetchType.EAGER 。

cascade:表示默认的级联操作策略，可以指定为 ALL 、 PERSIST 、 MERGE 、 REFRESH 和 REMOVE 中的若干组合，默认为无级联操作。

targetEntity:表示该属性关联的实体类型。该属性通常不必指定， ORM 框架根据属性类型自动判断 targetEntity。

示例:

```java
    //订单 Order 和用户 User 是一个 ManyToOne 的关系, 在 Order 类中定义
    @ManyToOne()
    @JoinColumn(name="USER")
    public User getUser() {

       return user;

    }
```

9、@JoinColumn

可选

@JoinColumn 和 @Column 类似，描述的不是一个简单字段，而是一个关联字段，例如描述一个 @ManyToOne 字段。

name:该字段的名称。由于 @JoinColumn 描述的是一个关联字段，如 ManyToOne ，则默认的名称由其关联的实体决定。

例如，实体 Order 有一个 user 属性来关联实体 User ，则 Order 的 user 属性为一个外键，其默认的名称为实体 User 的名称 + 下划线 + 实体 User 的主键名称。

10、@OneToMany(fetch=FetchType, cascade=CascadeType)

可选

@OneToMany 描述一个一对多的关联，该属性应该为集合类型，在数据库中并没有实际字段。

fetch:表示抓取策略，默认为 FetchType.LAZY ，因为关联的多个对象通常不必从数据库预先读取到内存。

cascade:表示级联操作策略，对于 OneToMany 类型的关联非常重要，通常该实体更新或删除时，其关联的实体也应当被更新或删除。

例如:实体 User 和 Order 是 OneToMany 的关系，则实体 User 被删除时，其关联的实体 Order 也应该被全部删除。

示例:

```java
    @OneTyMany(cascade=ALL)
    public List getOrders() {

       return orders;

    }
```

11、@OneToOne(fetch=FetchType, cascade=CascadeType)

可选

@OneToOne 描述一个一对一的关联。

fetch:表示抓取策略，默认为 FetchType.LAZY 。

cascade:表示级联操作策略。

示例:

```java
    @OneToOne(fetch=FetchType.LAZY)
    public Blog getBlog() {

       return blog;

    }
```

12、@ManyToMany

可选

@ManyToMany 描述一个多对多的关联。多对多关联上是两个一对多关联，但是在 ManyToMany 描述中，中间表是由 ORM 框架自动处理。

targetEntity:表示多对多关联的另一个实体类的全名，例如: package.Book.class .

mappedBy:表示多对多关联的另一个实体类的对应集合属性名称.

示例:

    User 实体表示用户，Book实体表示书籍，为了描述用户收藏的书籍，可以在 User 和 Book 之间建立 ManyToMany 关联。

```java
    @Entity
    public class User {

       private List books;

       @ManyToMany(targetEntity=package.Book.class)

       public List getBooks() {

           return books;

       }

       public void setBooks(List books) {

           this.books=books;

       }

    }
```

```java
    @Entity
    public class Book {

       private List users;

       @ManyToMany(targetEntity=package.Users.class, mappedBy="books")

       public List getUsers() {

           return users;

       }

       public void setUsers(List users) {

           this.users=users;

       }

    }
```

两个实体间相互关联的属性必须标记为 @ManyToMany ，并相互指定 targetEntity 属性，需要注意的是，有且只有一个实体的 @ManyToMany 注解需要指定mappedBy属性，指向 targetEntity 的集合属性名称。

利用 ORM 工具自动生成的表除了 User 和 Book 表外，还自动生成了一个 User_Book 表，用于实现多对多关联。

13、@MappedSuperclass

可选

@MappedSuperclass可以将超类的 JPA 注解传递给子类，使子类能够继承超类的JPA注解。 DomainEntity 就是使用的此标记。

示例:

```java
    @MappedSuperclass
    public class Employee() {

       ...

    }
```

```java
    @Entity
    public class Engineer extends Employee {

       ...

    }
```

```java
    @Entity
    public class Manager extends Employee {

       ...

    }
```

14、@Embedded

可选

@Embedded 将几个字段组合成一个类，并作实体的一个属性。

添加了 @Embedded 注解的实体必须实现 Serializable 接口。

例如 User 包括 id 、name 、city 、street 、zip 属性。

我们希望 city 、 street 、 zip 属性映射为 Address 对象。这样， User 对象将具有 id 、 name 和 address 这三个属性。

Address 对象必须定义为 @Embededable。

示例:

```java
    @Embeddable
    public class Address implements Serializable {

        private String city;
        private String street;
        private String zip;

        //// getters and setters
        ...

    }
```

```java
    @Entity
    public class User {

       @Embedded
       public Address getAddress() {

           ...

       }

    }
```