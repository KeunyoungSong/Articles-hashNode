---
title: "직렬화와 비직렬화: Serializable과 Parcelable의 이해"
datePublished: Mon May 27 2024 07:53:26 GMT+0000 (Coordinated Universal Time)
cuid: clwoo94y9000109l9hticfezh
slug: serializable-parcelable
tags: serialization, android

---

# 직렬화와 비직렬화 개념

## 직렬화(Serialization)

**직렬화는 객체의 상태를 바이트 스트림으로 변환하는 과정.**

> 바이트 스트림은 0과 1로 이루어진 데이터의 연속

바이트 스트림은 파일 저장, 네트워크 전송 등 다양한 목적을 위해 사용할 수 있다. 객체를 바이트 스트림으로 변환하면, 해당 객체의 모든 필드와 데이터가 연속된 바이트 형태로 저장되어 다른 시스템이나 프로세스에서도 이 데이터를 읽고 사용할 수 있다.

### 사용 예시

* 안드로이드에서 Intent 간 데이터 전송
    
* 객체를 파일에 저장하거나 데이터베이스에 저장할 때
    
* 네트워크를 통해 객체를 전송할 때
    

## 비직렬화(Deserialization)

비직렬화는 직렬화된 바이트 스트림을 다시 원래의 객체 상태로 변환하는 과정. 이를 통해 저장된 데이터를 다시 객체로 복원하여 사용할 수 있다. 바이트 스트림에서 객체로의 변환을 통해 데이터의 일관성과 재사용성을 유지할 수 있다.

## 바이트 스트림이 필요한 이유

플랫폼 간 호환성: 바이트 스트림을 사용하면 서로 다른 플랫폼이나 시스템 간에 데이터 교환이 가능해집니다.

# 안드로이드의 직렬화/비직렬화

Java 의 **Serializable**

Android 의 **Parcelable**

## Java의 직렬화 (Serializable 인터페이스)

Java에서 직렬화를 지원하는 가장 기본적인 방법은 `Serializable` 인터페이스를 구현하는 것. 이 인터페이스는 아무 메서드도 가지지 않으며, 단순히 클래스가 직렬화될 수 있음을 표시.

#### Serializable 사용 예제

Java 의 직렬화 메커니즘을 사용하여 객체를 바이트 스트림으로 변환하고, 이를 파일 시스템에 저장하고 복원하는 코드

```java
import java.io.*;

class Student implements Serializable {
    private static final long serialVersionUID = 1L;  // 클래스 버전 호환성 관리를 위한 클래스 버전 UID
    private String name;
    private int age;

    // 생성자, getter, setter

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{name='" + name + "', age=" + age + "}";
    }
}

public class SerializationExample {
    public static void main(String[] args) {
        Student student = new Student("John", 20);

        // 객체 직렬화
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("student.ser"))) {
            oos.writeObject(student);
            System.out.println("Object has been serialized: " + student);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 객체 비직렬화
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("student.ser"))) {
            Student deserializedStudent = (Student) ois.readObject();
            System.out.println("Object has been deserialized: " + deserializedStudent);
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

위 코드는 Student 클래스에 Java의 Serializable를 구현하여 객체를 직렬화하고 비직렬화 하여 복원하는 예시 코드이다.

스트림 체인 방식으로 구현되었으며

직렬화 시, `FileOutputStream` 가 먼저 `student.ser` 파일을 생성하고 `ObjectOutputStream` 가 student 객체를 바이트 스트림으로 변환해 이를 `FileOutputStream` 에 전달하여 파일에 작성한다.

비직렬화 시, `FileInputStream` 가 먼저 `student.ser` 파일을 열고 이후 `ObjectInputStream` 를 통해 바이트 스트림 데이터를 객체로 복원한다.

## Android의 직렬화 (Parcelable 인터페이스)

Android에서는 객체의 직렬화 및 비직렬화에 있어 `Serializable`보다 `Parcelable` 인터페이스를 사용하는 것이 권장됨.

#### Parcelable 사용 예제

Java의 `Serializable` 대신 Android 의 `Parcelable` 인터페이스를 구현하여 Intent 간 데이터 전달에 사용하는 코드

```java
import android.os.Parcel;
import android.os.Parcelable;

public class Student implements Parcelable {
    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Parcel에서 데이터를 읽어오는 생성자
    protected Student(Parcel in) {
        name = in.readString();
        age = in.readInt();
    }

    // 객체를 Parcel에 쓰는 메서드
    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeString(name);
        dest.writeInt(age);
    }

    // 특수한 내용이 있는지 여부를 설명 (일반적으로 0 반환)
    @Override
    public int describeContents() {
        return 0;
    }

    // Parcelable.Creator 인터페이스 구현
    public static final Parcelable.Creator<Student> CREATOR = new Parcelable.Creator<Student>() {
        @Override
        public Student createFromParcel(Parcel in) {
            return new Student(in);
        }

        @Override
        public Student[] newArray(int size) {
            return new Student[size];
        }
    };

    @Override
    public String toString() {
        return "Student{name='" + name + "', age=" + age + "}";
    }
}
```

인텐트를 통해 Student 객체를 다른 Activity 에 전달하는 코드

```java
import android.content.Intent;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Student student = new Student("John", 20);

        Intent intent = new Intent(this, SecondActivity.class);
        intent.putExtra("student", student);
        startActivity(intent);
    }
}

// SecondActivity.java
import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class SecondActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        Student student = getIntent().getParcelableExtra("student");

        TextView textView = findViewById(R.id.textView);
        textView.setText(student.toString());
    }
}
```

### Serializable vs Parcelable

#### Serializable

장점

* Java 표준 라이브러리를 사용
    

단점

* Android에서 상대적으로 느리고 메모리 사용이 많음
    
* 상대적으로 더 많은 오버헤드
    

#### **Parcelable**

장점

* Android에서 더 빠르고 효율적
    
* 인텐트 간 데이터 전달에 최적화
    

단점

* 구현이 다소 복잡(**@Parcelize** 을 통해 개선됨)
    
* Java 표준 라이브러리가 아니기 때문에 다른 플랫폼에서는 사용할 수 없음
    

## 두 기술의 차이가 생기는 이유

* Serializable: Java의 기본 직렬화 방식인 `Serializable` 은 리플렉션을 사용하여 객체의 모든 필드를 직렬화 한다. 리플렉션은 유연하지만 성능이 떨어지고 메모리 사용량이 많다. 이는 Java 표준 라이브러리로 구현되었기 때문에 코드 이식성이 높지만, Android 환경에서는 성능상의 단점이 있다.
    
* Parcelable: Android의 `Parcelabe` 은 직렬화를 위해 직접 구현된 메서드를 사용한다. 이는 메타데이터를 포함하지 않기 때문에 더 효율적이고 빠르다. `Parcelable` 은 Android 프레임워크의 일부로 설계되어 Android의 메모리 및 성능 특성을 고려하여 최적화되어 있다. 그러나 구현이 복합하고 Java 표준이 아니기 때문에 다른 플랫폼에서는 사용할 수 없다.
    

---

# 참고

## Java 직렬화의 기본 원리

1. 객체의 클래스 정보 저장: 직렬화된 데이터는 객체의 클래스 정보를 포함한다. 여기에는 클래스 이름, serialVersionUID, 필드 정보 등이 포함됨
    
2. 필드 값 저장: 객체의 각 필드 값이 바이트 스트림으로 변환되어 저장된다. 필드의 타입에 따라 변환 방식이 다르다. (예: 정수, 문자열, 객체 참조 등)
    

## 직렬화 포멧

Java 직렬화 포맷은 특정 규칙을 따릅니다. 여기에는 Stream Magic, Stream Version, 클래스 설명 및 객체 데이터가 포함됩니다. 다음은 주요 포맷 요소들입니다:

1. **Stream Header**:
    
    * `Stream Magic`: 직렬화 스트림의 시작을 나타내는 고정 값 (`0xACED`).
        
    * `Stream Version`: 직렬화된 스트림의 버전 (`0x0005`).
        
2. **클래스 정보**:
    
    * 클래스 이름, serialVersionUID, 필드 정보 등이 포함됩니다.
        
3. **객체 데이터**:
    
    * 객체의 필드 값이 바이트 스트림으로 변환되어 저장됩니다.
        

### **예시: Student 객체 직렬화**

```java
import java.io.*;

class Student implements Serializable {
    private static final long serialVersionUID = 1L;  // 직렬화 버전 UID
    private String name;
    private int age;

    // 생성자, getter, setter

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{name='" + name + "', age=" + age + "}";
    }
}

public class SerializationExample {
    public static void main(String[] args) {
        Student student = new Student("John", 20);

        // 객체 직렬화
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("student.ser"))) {
            oos.writeObject(student);
            System.out.println("Object has been serialized: " + student);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 직렬화된 파일의 바이트 데이터 출력
        try (FileInputStream fis = new FileInputStream("student.ser")) {
            int data;
            System.out.print("Serialized data in hexadecimal: ");
            while ((data = fis.read()) != -1) {
                System.out.printf("%02X ", data);
            }
            System.out.println();
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 객체 비직렬화
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("student.ser"))) {
            Student deserializedStudent = (Student) ois.readObject();
            System.out.println("Object has been deserialized: " + deserializedStudent);
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

위 코드를 통해 직렬화된 바이트 스트림 데이터를 출력해봤다.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716795740912/d7abf9c1-1983-4468-a8ed-46b26c11c5c4.png align="center")

```java
AC ED 00 05 73 72 00 07 53 74 75 64 65 6E 74 B1 6D E3 3C 0A 29 D3 0C 00 00 02 00 02 49 00 03 61 67 65 4C 00 04 6E 61 6D 65 74 00 12 4C 6A 61 76 61 2F 6C 61 6E 67 2F 53 74 72 69 6E 67 3B 78 70 00 00 00 14 74 00 04 4A 6F 68 6E
```

직렬화된 데이터 분석 상세 설명

직렬화된 `student.ser` 파일의 바이트 데이터를 하나하나 분석해 보겠습니다. 직렬화된 데이터는 특정 포맷을 따르며, 이를 이해하려면 Java 직렬화 스트림의 구조를 알아야 합니다.

#### 직렬화된 데이터 (16진수):

```java
AC ED 00 05 73 72 00 07 53 74 75 64 65 6E 74 B1 6D E3 3C 0A 29 D3 0C 00 00 02 00 02 49 00 03 61 67 65 4C 00 04 6E 61 6D 65 74 00 12 4C 6A 61 76 61 2F 6C 61 6E 67 2F 53 74 72 69 6E 67 3B 78 70 00 00 00 14 74 00 04 4A 6F 68 6E
```

이를 다음과 같이 해석할 수 있습니다:

### 1\. Stream Header (스트림 헤더)

```java
AC ED 00 05
```

* **AC ED**: Stream Magic (Java 직렬화 스트림의 시작을 나타냄)
    
* **00 05**: Stream Version (Java 직렬화 스트림의 버전)
    

### 2\. Class Descriptor (클래스 설명)

```java
73 72 00 07 53 74 75 64 65 6E 74 B1 6D E3 3C 0A 29 D3 0C 00 00 02 00 02
```

* **73**: TC\_OBJECT (객체의 시작)
    
* **72**: TC\_CLASSDESC (클래스 설명의 시작)
    
* **00 07**: 클래스 이름의 길이 (7바이트)
    
* **53 74 75 64 65 6E 74**: 클래스 이름 (`Student`의 ASCII 코드)
    
* **B1 6D E3 3C 0A 29 D3 0C**: serialVersionUID (클래스의 고유 ID)
    
* **00 00**: 클래스의 필드 수 (필드 설명의 시작, 2개 필드가 있음)
    
* **02**: 필드 수 (2개 필드가 있음)
    

### 3\. Field Descriptors (필드 설명)

#### 첫 번째 필드

```java
49 00 03 61 67 65
```

* **49**: 필드 타입 (`I`는 int를 의미)
    
* **00 03**: 필드 이름의 길이 (3바이트)
    
* **61 67 65**: 필드 이름 (`age`의 ASCII 코드)
    

#### 두 번째 필드

```java
4C 00 04 6E 61 6D 65 74 00 12 4C 6A 61 76 61 2F 6C 61 6E 67 2F 53 74 72 69 6E 67 3B
```

* **4C**: 필드 타입 (`L`은 객체를 의미)
    
* **00 04**: 필드 이름의 길이 (4바이트)
    
* **6E 61 6D 65**: 필드 이름 (`name`의 ASCII 코드)
    
* **74 00 12**: 필드 타입의 길이 (18바이트, `Ljava/lang/String;`)
    
* **4C 6A 61 76 61 2F 6C 61 6E 67 2F 53 74 72 69 6E 67 3B**: 필드 타입 (`Ljava/lang/String;`)
    

### 4\. Class Termination (클래스 종료)

```java
78 70
```

* **78**: TC\_ENDBLOCKDATA (클래스 설명 종료)
    
* **70**: TC\_NULL (클래스가 없음을 의미)
    

### 5\. Object Data (객체 데이터)

#### 첫 번째 필드 데이터 (int 타입의 `age`)

```java
00 00 00 14
```

* **00 00 00 14**: `age`의 값 (20, 정수형 데이터로 4바이트)
    

#### 두 번째 필드 데이터 (String 타입의 `name`)

```java
74 00 04 4A 6F 68 6E
```

* **74**: TC\_STRING (문자열의 시작)
    
* **00 04**: 문자열 길이 (4바이트)
    
* **4A 6F 68 6E**: 문자열 값 (`John`의 ASCII 코드)
    

### 요약

* **AC ED**: Stream Magic
    
* **00 05**: Stream Version
    
* **73**: TC\_OBJECT (객체의 시작)
    
* **72**: TC\_CLASSDESC (클래스 설명의 시작)
    
* **00 07**: 클래스 이름의 길이 (7바이트)
    
* **53 74 75 64 65 6E 74**: 클래스 이름 (`Student`)
    
* **B1 6D E3 3C 0A 29 D3 0C**: serialVersionUID
    
* **00 00 02**: 클래스의 필드 수 (2개 필드)
    
* **49 00 03 61 67 65**: 첫 번째 필드 (정수형 `age`)
    
* **4C 00 04 6E 61 6D 65 74 00 12 4C 6A 61 76 61 2F 6C 61 6E 67 2F 53 74 72 69 6E 67 3B**: 두 번째 필드 (문자열 `name`)
    
* **78 70**: 클래스 종료
    
* **00 00 00 14**: `age` 값 (20)
    
* **74 00 04 4A 6F 68 6E**: `name` 값 (`John`)
    

이렇게 각 16진수 값이 직렬화된 데이터의 구조와 내용을 나타냅니다. Java의 직렬화 메커니즘을 통해 객체가 바이트 스트림으로 변환되며, 이를 통해 객체의 상태를 저장하거나 네트워크로 전송할 수 있습니다.