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
        M("Master"), 
        S("Slave"), 
        BLANK(""),;

        private static final String ERR_MSG_FMT_FOR_ILLEGAL_STATE_EX;
        private final String value;

        static {
            ERR_MSG_FMT_FOR_ILLEGAL_STATE_EX = String.format(
                "'%s' は Enum %s で定義されていない値です", 
                "%s",
                MasterSlaveType.class.getName()
            );
        }

        private MasterSlaveType(String value) {
            this.value = value;
        }

        private String value() {
            return this.value;
        }

        return Arrays.stream(MasterSlaveType.values())
                    .filter(type -> type.value().equals(value))
                    .findFirst()
                    .orElseThrow(() -> {
                        throw new IllegalStateException(
                            String.format(
                                ERR_MSG_FMT_FOR_ILLEGAL_STATE_EX,
                                value
                            )
                        );
                    });
    }

    public static void main(String[] args) {

        String[] words = { 
            "Master", "Slave", "", "Customrer", 
        };

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

        private static final String ERR_MSG_FMT_FOR_ILLEGAL_STATE_EX;
        private static final Map<String, MasterSlaveType> MAPPING;
        private final String value;

        static {
            ERR_MSG_FMT_FOR_ILLEGAL_STATE_EX = String.format(
                    "'%s' は Enum %s で定義されていない値です", "%s",
                    MasterSlaveType.class.getName());

            mapping = new HashMap<>();
            Arrays.stream(MasterSlaveType.values())
                    .forEach(type -> MAPPING.put(type.value(), type));
        }

        private MasterSlaveType(String value) {
            this.value = value;
        }

        private String value() {
            return this.value;
        }

        public static MasterSlaveType fromValue(String value) {
            MasterSlaveType result = MAPPING.get(value);
            if (result == null) {
                throw new IllegalStateException(
                    String.format(
                        ERR_MSG_FMT_FOR_ILLEGAL_STATE_EX, 
                        value
                    )
                );
            }
            return result;
        }
    }

    public static void main(String[] args) {

        String[] words = { 
            "Master", "Slave", "", "Customrer", 
        };

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
