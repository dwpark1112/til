# Replace Type Code with Strategy pattern

리팩토링 안하면서 디자인패턴 적용한다는 건 매우 어렵다. 리팩토링을 자주 하다보면 자연스레 패턴이 적용되는 상황이 발생하고, 딱 필요한 시점에 패턴을 적용할 때 개발이 참 재미있어진다.

자바에서 Type같은 것들로 인해 클래스 행위가 바뀔 때, 보통은 switch-case로 구현하게 된다. 그런데 switch-case로 두는 것보다 이것을 strategy pattern을 적용해서 풀어내는게 훨씬 좋다. 반대로 switch-case가 많이 늘어날 것 같지 않으면 그냥 switch-case로 두는게 낫다. 뭐든 상황에 따라...

ref <https://www.refactoring.com/catalog/replaceTypeCodeWithStateStrategy.html>


## Before 

아래처럼 Employee 클래스에서는 함수가 추가될 때마다 switch-case를 반복해서 사용해야 하고, employee타입이 추가 될 때는 모든 함수를 조금씩 수정해주어야만 한다. 애초에 저런건 그냥 서브 클래스를 만들어서 처리해도 되니깐 사실 예가 별로 좋진 않다. 

```java
class Employee {
	static final int ENGINEER = 0;
	static final int MANAGER = 1;

	static final int DEFAULT_FLEXTIME_HOUR = 10;

	public int type;
    public int monthlySalary;

	public Employee(int type, int monthlySalary) {
		this.type = type;
		this.monthlySalary = monthlySalary;
	}

	public int payAmount() {
		switch (type) {
			case ENGINEER :
				return monthlySalary * 2;
			case MANAGER :
				return monthlySalary;
			default :
				throw new RuntimeException("Incorrect Employee Code");
		}
	}

	public int getFlexTime() {
		switch (type) {
			case ENGINEER :
				return DEFAULT_FLEXTIME_HOUR * 2;
			case MANAGER :
				return DEFAULT_FLEXTIME_HOUR + 1;
			default :
				throw new RuntimeException("Incorrect");
		}
	}

	public static void main(String[] args) {
		int employeeType = Employee.ENGINEER;
        int monthlySalary = 2000;

		Employee employee = new Employee(employeeType, monthlySalary);
		employee.payAmount();
		employee.getFlexTime();
	}
}
```

## After

클래스가 무진장 늘어나지만, 나중에 employee 타입이 추가되더라도 switch-case를 수정해야 하는 부분은 하나면 된다. 특히, Open-Close principle을 잘 지킬 수 있어서 좋다. 

```java
public class MyEmployee {
	public static final int DEFAULT_FLEXTIME_HOUR = 10;

	private EmployeeType employeeType;
	private int monthlySalary;

	public MyEmployee(EmployeeType.Category category, int monthlySalary) {
		this.employeeType = EmployeeType.newInstance(category);
		this.monthlySalary = monthlySalary;
	}

	public int getMonthlySalary() {
		return monthlySalary;
	}

	public int getFlexTime() {
		return this.employeeType.getFlexTime(this);
	}

	public int payAmount() {
		return this.employeeType.payAmount(this);
	}

    public static void main(String[] args) {
        EmployeeType.Category category = EmployeeType.Category.ENGINEER;
        int monthlySalary = 2000;

		MyEmployee myEmployee = new MyEmployee(category, monthlySalary);
		myEmployee.payAmount();
		myEmployee.getFlexTime();
    }
}
```
```java
// EmployeeType
public abstract class EmployeeType {

    public abstract int getFlexTime(MyEmployee employee);
    public abstract int payAmount(MyEmployee employee);

	public static EmployeeType newInstance(Category category) {
		EmployeeType employeeType;
		switch (category) {
			case ENGINEER :
				employeeType = new EngineerType();
				break;
			case MANAGER :
				employeeType = new ManagerType();
				break;
			default :
				throw new IllegalArgumentException();
		}

		return employeeType;
	}

	public enum Category {
		ENGINEER, MANAGER;
	}
}
```
```java
// EngineerType
public class EngineerType extends EmployeeType {
	@Override
	public int getFlexTime(MyEmployee employee) {
		return employee.DEFAULT_FLEXTIME_HOUR * 2;
	}

	@Override
	public int payAmount(MyEmployee employee) {
		return employee.getMonthlySalary() * 2;
	}
}
```
```java
// ManagerType
public class ManagerType extends EmployeeType {
    @Override
    public int getFlexTime(MyEmployee employee) {
        return employee.DEFAULT_FLEXTIME_HOUR + 1;
    }

    @Override
    public int payAmount(MyEmployee employee) {
        return employee.getMonthlySalary();
    }
}

```


## 회고

개발할 때 switch-case가 많이 발생하게 되는 상황은 생각보다 많았다. 비용 계산할 때 또는 통계데이터를 뽑을 때, date range 설정하는 용도로 필요했다. 일일/주간/월간/분기/년간 이런식으로 query문에서 date range가 필요할 때, date range를 만드는 클래스를 방법으로 처리하면 일이 편해진다. 재미는 보너스