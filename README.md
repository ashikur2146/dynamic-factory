# Dynamic Factory

Factory design pattern is one of the most used, perhaps the most used design pattern in the software development. For a small scale project the downside of this pattern is not easily understood, but in a large scale project where too many functional components co-exist, branching for this pattern really become cumbersome. So, I am publishing a library named dynamic factory which will take care most of this cumbersome work. It is published under General Public License (GPL v3.0).

You can also extend its functionality and make it serve your needs within its boundary. Just a disclaimer before diving into the how to guide, this library is not available on maven central so follow the steps to add this into your current project.

# Signature verification

This library is digitally signed. In order to verify its authenticity openup a terminal within the directory and run the following command

```
jarsigner -verify dynamic-factory-1.0.0.jar
```

You will find the output similar to below if it the library is verified, please note that certificate was not issued by any trusted Certificate authority.

```
jar verified.
```
....

# How to guide

1. clone the project on your local machine with git clone command
2. navigate to the project directory
3. inside the project directory you will find the DYNAMIC-FACTORY-1.0.0.jar
4. copy the jar file
5. create a folder location inside your project's resource directory, for example, /src/resources/libs/
6. paste the jar file inside your created directory
7. it is assumed that your are using a build tool for your project i.e. maven or gradle
8. if so, then add the dependency path inside your pom.xml for maven, or build.gradle file for a gradle project. In case you are not using any build tool just add the jar file as an external dependency to your project
9. update the project
10. you are good to go!

# Add as a maven dependency

```maven
<dependency>
	<groupId>approval.process</groupId>
	<artifactId>approval-process</artifactId>
	<version>1.0.0</version>
	<scope>system</scope>
	<systemPath>${project.basedir}/src/main/resources/libs/DYNAMIC-FACTORY-1.0.0.jar</systemPath>
</dependency>
```

# Add as a gradle dependency

```gradle
dependencies {
    implementation(files("/src/main/resources/libs/DYNAMIC-FACTORY-1.0.0.jar"))
}
```


# Example code

The following code contains different business logic implementation 

```java
public class Another {

	private int x = 5;

	public int getA(Integer p) {
		return 14 * p;
	}

	public int getA() {
		return 25;
	}

	public int getAB() {
		return 35;
	}

	public int getB() {
		return 10;
	}

	public int getC() {
		return 5;
	}

	public int getX() {
		return x;
	}

	public void setX(int x) {
		this.x = x;
	}

	public void getVoid() {
		System.out.println("test void");
	}
}
```
The following code snippet is the example of how to achieve the dynamic factory behavior

```java
import java.util.ArrayList;
import java.util.List;

import dynamic.factory.DataFetcher;
import dynamic.factory.EnumValidator;
import dynamic.factory.ParamModel;

public class ValidationTest {
	public static void main(String[] args) throws Exception {
		TestEnum s = new EnumValidator<TestEnum>().getEnumConstant(TestEnum.class, TestEnum.B);
		System.out.println(s.getValue());
		Integer n = new DataFetcher<Integer>().getValidatedData(Another.class, s.getValue(), null, null, null);
		System.out.println(n);

		TestEnum s1 = new EnumValidator<TestEnum>().getEnumConstant(TestEnum.class, TestEnum.A);
		System.out.println(s1.getValue());
		List<ParamModel> paramMappingList = new ArrayList<ParamModel>();
		paramMappingList.add(new ParamModel(Integer.class, 2));
		Integer n1 = new DataFetcher<Integer>().getValidatedData(test.Another.class, s1.getValue(), paramMappingList, null, null);
		System.out.println(n1);

		TestEnum s4 = new EnumValidator<TestEnum>().getEnumConstant(TestEnum.class, TestEnum.A);
		System.out.println(s4.getValue());
		Integer n4 = new DataFetcher<Integer>().getValidatedData(test.Another.class, s4.getValue(), null, null, null);
		System.out.println(n4);

		TestEnum s5 = new EnumValidator<TestEnum>().getEnumConstant(TestEnum.class, TestEnum.AB);
		System.out.println(s5.getValue());
		Integer n5 = new DataFetcher<Integer>().getValidatedData(test.Another.class, s5.getValue(), null, null, null);
		System.out.println(n5);
		
		TestEnum s2 = new EnumValidator<TestEnum>().getEnumConstant(TestEnum.class, TestEnum.X);
		System.out.println(s2.getValue());
		//List<ParamMappingModel> paramMappingList = new ArrayList<ParamMappingModel>();
		Another anotherObj = new Another();
		anotherObj.setX(20);
		Integer n2 = new DataFetcher<Integer>().getValidatedData(test.Another.class, s2.getValue(), null, anotherObj, null);
		System.out.println(n2);
		
		TestEnum s3 = new EnumValidator<TestEnum>().getEnumConstant(TestEnum.class, TestEnum.VOID);
		new DataFetcher<Void>().getValidatedData(Another.class, s3.getValue(), null, null, null);
	}

	enum TestEnum {
		A("a"), B("b"), AB("ab"), C("c"), X("x"), VOID("void");

		private final String value;

		private TestEnum(String value) {
			this.value = value;
		}

		public String getValue() {
			return value;
		}
	}
}
```
output:
```
b
10
a
28
a
25
ab
35
x
20
test void
```

# Integration with other programming languages

This library could be compatible with other programming languages also through the provided java support library of the language.
For example, 

* py4J provides java support for python programmers (explore here: https://www.py4j.org/)
* java support for php developers (https://github.com/php-java/php-java)

# What's Next

If you find this library helpful by any chance and want to integrate in your project but need assistance, then feel free to book an appointment at ashikur2146@gmail.com
