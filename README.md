# enum-reverse-lookup
文字列から Enum を逆引きする方法

## STREAM API を使う方法

``` java
import java.util.Arrays;

public class App {

    /**
     * 主従区分
     */
    public enum MasterSlaveType {
        M("Master"), S("Slave"), BLANK(""),;

        private static final String ERR_MSG_FMT_FOR_ILLEGAL_STATE_EX;
        private final String value;

        static {
            ERR_MSG_FMT_FOR_ILLEGAL_STATE_EX = String.format(
                    "'%s' は Enum %s で定義されていない値です", "%s",
                    MasterSlaveType.class.getName());
        }

        private MasterSlaveType(String value) {
            this.value = value;
        }

        private String value() {
            return this.value;
        }

        private static MasterSlaveType fromValue(String value) {
            return Arrays.stream(MasterSlaveType.values())
                    .filter(type -> type.value().equals(value)).findFirst()
                    .orElseThrow(() -> {
                        throw new IllegalStateException(String.format(
                                ERR_MSG_FMT_FOR_ILLEGAL_STATE_EX, value));
                    });
        }
    }

    public static void main(String[] args) {

        String[] words = { "Master", "Slave", "", "Customrer", };

        for (String word : words) {
            printMasterSlaveTypeFromWord(word);
        }
    }

    private static void printMasterSlaveTypeFromWord(String word) {
        MasterSlaveType type = MasterSlaveType.fromValue(word);
        System.out.println(type);
    }
}
```

`実行結果: `
``` console
M
S
BLANK
Exception in thread "main" java.lang.IllegalStateException: 'Customrer' は Enum App$MasterSlaveType で定義されていない値です
```

## Map を使う方法

``` java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class App {

    /**
     * 主従区分
     */
    public enum MasterSlaveType {
        M("Master"), 
        S("Slave"), 
        BLANK(""),;

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
                throw new IllegalStateException(
                    String.format(
                        "'%s' は MasterSlaveType で定義されていない列挙子です", 
                        value));
            }
            return result;
        }
    }

    public static void main(String[] args) {

        MasterSlaveType typeMaster = MasterSlaveType.fromValue("Master");
        MasterSlaveType typeSlave = MasterSlaveType.fromValue("Slave");
        MasterSlaveType typeBlank = MasterSlaveType.fromValue("");
        MasterSlaveType typeCustomer = MasterSlaveType.fromValue("Customer");

        System.out.println(typeMaster);
        System.out.println(typeSlave);
        System.out.println(typeBlank);
        System.out.println(typeCustomer);
    }
}
```
