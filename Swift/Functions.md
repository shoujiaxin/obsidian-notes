## 派发机制

### 直接派发 (Direct Dispatch)

### 函数表派发 (Table Dispatch)

### 消息机制派发 (Message Dispatch)

|                        | 直接派发                      | 函数表派发           | 消息派发           |
| ---------------------- | ----------------------------- | -------------------- | ------------------ |
| struct                 | all                           | N/A                  | N/A                |
| class                  | extensions, `final`, `static` | initial declarations | `@objc`, `dynamic` |
| protocol               | extensions                    | initial declarations | N/A                |
| subclass of `NSObject` | extensions, `final`, `static` | initial declarations | `@objc`, `dynamic` |
