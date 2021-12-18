# enum-reverse-lookup
文字列から Enum を逆引きする方法

## 

``` java
import java.util.HashMap;
import java.util.Map;

public class App {

    /**
     * 主従区分
     */
    public enum MasterSlaveType {
        M("M"), S("S"), BLANK(""),;

        private final String value;
        private static Map<String, MasterSlaveType> mapping = new HashMap<>();

        static {
            Arrays.stream(MasterSlaveType.values())
                    .forEach(type -> mapping.put(type.value(), type));
        }

        private MasterSlaveType(String value) {
            this.value = value;
        }

        private String value() {
            return this.value;
        }

        public static MasterSlaveType fromValue(final String value) {
            MasterSlaveType result = mapping.get(value);
            if (result == null) {
                throw new IllegalStateException(String
                        .format("'%s' は MasterSlaveType で定義されていない列挙子です", value));
            }
            return result;
        }
    }

    public static void main(String[] args) {

        MasterSlaveType typeM = MasterSlaveType.fromValue("M");
        MasterSlaveType typeS = MasterSlaveType.fromValue("S");
        MasterSlaveType typeBlank = MasterSlaveType.fromValue("C");
        
        System.out.println(typeM.value);
        System.out.println(typeS.value);
        System.out.println(typeBlank.value);
    }
}
```

## 

``` java
import java.util.Arrays;

public class App {

    /**
     * 主従区分
     */
    public enum MasterSlaveType {
        M("M"), S("S"), BLANK(""),;

        private final String value;

        private MasterSlaveType(String value) {
            this.value = value;
        }

        private String value() {
            return this.value;
        }

        private static MasterSlaveType fromValue(String value) {
            return Arrays.stream(MasterSlaveType.values())
                    .filter(type -> type.value().equals(value))
                    .findFirst()
                    .orElseThrow(() -> {
                        throw new IllegalStateException(
                            String.format(
                                "'%s' は MasterSlaveType で定義されていない列挙子です",
                                value));
                    });
        }
    }

    public static void main(String[] args) {

        MasterSlaveType typeMaster = MasterSlaveType.fromValue("M");
        MasterSlaveType typeSlave = MasterSlaveType.fromValue("S");
        MasterSlaveType typeCustomer = MasterSlaveType.fromValue("C");

        System.out.println(typeMaster.value);
        System.out.println(typeSlave.value);
        System.out.println(typeCustomer.value);
    }
}
```
