# encoding vs serialisation?

Encoding: Translating data from one form to another. Only data can be encoded, if anyting other than data has to be encoded, it will have to be converted to data. Encoded data can be decoded anywhere if the encoding is known.

Serialisation: Saving of an entire object with all of its methods, attributes, and their relationships along with data. Serialisation is only useful/readable to the language the serialised file was from.

# research the various method to convert an object in memory to a file and then read that file back into an object in

While it is possible to do serialisation manually, it is usually better to use external libraries to do this for us. This is because they have been optimised as much as possible, are known to be reliable, and most people who need to serialise an object don't want to write their own implementation for it.

## C++

Here we are uding boost serialisation library.

```cpp
#include <fstream>
#include <boost/archive/text_iarchive.hpp>
#include <boost/archive/text_oarchive.hpp>

using namespace std;

class Employee {
private:
   friend class boost::serialization::access;
   int id;
   string name;
   float salary;
   template<class Archive>
   void serialize(Archive &a, const unsigned version){
      a & id & name & salary;
   }
public:
   Employee(){}
   Employee(int i, string n, float s):id(i),name(n),salary(s)
   {}
};

int main()
{
   const string filename = "SaveFile";
   Employee e1(11,"Harry",4500.00f);
   Employee e2(22,"Ravi",8800.00f);
   Employee e3(33,"Tim",6800.00f);
   Employee e4(44,"Rajiv",3400.00f);

   // Serialize and persist the object
   {
      std::ofstream outfile(filename);
      boost::archive::text_oarchive archive(outfile);
      archive << e1 << e2 << e3 << e4;
   }

   // Deserialize and restore the object
   Employee restored_e1;
   Employee restored_e2;
   Employee restored_e3;
   Employee restored_e4;

   {
      std::ifstream infile(filename);
      boost::archive::text_iarchive archive(infile);
      archive >> restored_e1 >> restored_e2
         >> restored_e3 >> restored_e4;
   }
   return 0;
}
```

# Python

Using pickle in python

```python
import pickle

class Employee:
    def __init__(self, id=0, name='', salary=0.0):
        self.id = id
        self.name = name
        self.salary = salary

    def __repr__(self):
        return f"Employee(id={self.id}, name='{self.name}', salary={self.salary})"

# Creating Employee objects
e1 = Employee(11, "Harry", 4500.00)
e2 = Employee(22, "Ravi", 8800.00)
e3 = Employee(33, "Tim", 6800.00)
e4 = Employee(44, "Rajiv", 3400.00)

filename = 'SaveFile.pkl'

# Serialize and persist the objects
with open(filename, 'wb') as outfile:
    pickle.dump([e1, e2, e3, e4], outfile)

# Deserialize and restore the objects
with open(filename, 'rb') as infile:
    restored_employees = pickle.load(infile)

# Printing the restored objects to verify
for emp in restored_employees:
    print(emp)

```

# Java

Java has built in serialisation

```java
import java.io.*;

class Employee implements Serializable {
    private static final long serialVersionUID = 1L;
    
    private int id;
    private String name;
    private float salary;

    public Employee(int id, String name, float salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    @Override
    public String toString() {
        return "Employee{id=" + id + ", name='" + name + "', salary=" + salary + "}";
    }
}

public class Main {
    public static void main(String[] args) {
        Employee e1 = new Employee(11, "Harry", 4500.00f);
        Employee e2 = new Employee(22, "Ravi", 8800.00f);
        Employee e3 = new Employee(33, "Tim", 6800.00f);
        Employee e4 = new Employee(44, "Rajiv", 3400.00f);

        String filename = "SaveFile.ser";

        // Serialize and persist the objects
        try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream(filename))) {
            out.writeObject(e1);
            out.writeObject(e2);
            out.writeObject(e3);
            out.writeObject(e4);
        } catch (IOException i) {
            i.printStackTrace();
        }

        // Deserialize and restore the objects
        Employee restored_e1 = null;
        Employee restored_e2 = null;
        Employee restored_e3 = null;
        Employee restored_e4 = null;

        try (ObjectInputStream in = new ObjectInputStream(new FileInputStream(filename))) {
            restored_e1 = (Employee) in.readObject();
            restored_e2 = (Employee) in.readObject();
            restored_e3 = (Employee) in.readObject();
            restored_e4 = (Employee) in.readObject();
        } catch (IOException i) {
            i.printStackTrace();
        } catch (ClassNotFoundException c) {
            c.printStackTrace();
        }

        // Printing the restored objects to verify
        System.out.println(restored_e1);
        System.out.println(restored_e2);
        System.out.println(restored_e3);
        System.out.println(restored_e4);
    }
}
```

# 500 word short note on python pickle

Pickle is a python library for serialisation. Pickle converts an object into a byte stream that contains all the necessary information to reconstruct the object in another Python session. There are 6 protocols in pickle. with each version adding new features and optimizations. Here is a simplification of the opcodes.

* MARK ('('): Indicates the start of a new object or container.
* STOP ('.'): Indicates the end of the pickle stream.
* POP ('0'): Removes the topmost item from the stack.
* BINPUT ('q'): Used for storing memoized objects.
* LONG_BINPUT ('r'): Like BINPUT, but for large integers.
* NONE ('N'): Represents the Python None object.
* INT ('I'): Represents an integer.
* BININT ('J'): Represents a binary integer.
* BINSTRING ('T'): Represents a string.
* BINUNICODE ('X'): Represents a Unicode string.

Using given example

```python
import pickle

arr = bytes([
    0x80, 0x04, 0x95, 0x1A, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x5D,
    0x94, 0x28, 0x4B, 0x01, 0x4B, 0x02, 0x7D, 0x94, 0x8C, 0x03, 0x63, 0x61,
    0x74, 0x94, 0x8C, 0x05, 0x6D, 0x6F, 0x75, 0x73, 0x65, 0x94, 0x73, 0x65,
    0x2E
])

unpickled_object = pickle.loads(arr)

print(unpickled_object)
```

Gives

`[1, 2, {'cat': 'mouse'}]`

Explaination: 

https://raw.githubusercontent.com/python/cpython/main/Lib/pickletools.py

For eg `0x80` means we are using pickle protocal greater than 2.
`0x95` Indicates the beginning of a new frame.
and so on.

```
\x80\x04: Indicates that this pickle is using protocol 4.
\x95\x1A\x00\x00\x00\x00\x00\x00\x00: Frame header indicating the length of the following data (0x1A = 26 bytes).
\x5D: ] (MARK) opcode indicating the start of a list.
\x94: STOP opcode.
\x28: ( (TUPLE) opcode indicating the start of a tuple.
\x4B\x01: Integer 1.
\x4B\x02: Integer 2.
\x7D: } (DICT) opcode indicating the start of a dictionary.
\x94: STOP opcode.
\x8C\x03\x63\x61\x74: Unicode string 'cat'.
\x94: STOP opcode.
\x8C\x05\x6D\x6F\x75\x73\x65: Unicode string 'mouse'.
\x94: STOP opcode.
\x73: s (SETITEM) opcode indicating setting a dictionary item.
\x65: e (APPENDS) opcode for appending multiple items.
\x2E: . (STOP) opcode indicating the end of the pickle stream.
```
