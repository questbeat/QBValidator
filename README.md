# QBValidator
A library for validating values easily.


## Example
The most simple way is using `validateValues:rules:errorMessages:` of `QBValidator`.  
`values` is the pairs of key and value, `rules` is the pairs of key and rule array.

This example will fail.  
Let's change the values to pass all tests :)

    QBValidator *validator = [QBValidator validator];
    
    NSDictionary *errorMessages = nil;
    BOOL isValid = [validator validateValues:@{
                                               @"key1": @"suzuya",
                                               @"key2": @"agano",
                                               @"key3": @(58),
                                               @"key4": @(168),
                                               @"key5": @"abc+123",
                                               @"key6": @"no.body..@example.com"
                                               }
                                       rules:@{
                                               @"key1": @[QBVRequired, QBVEqualTo(@"kumano")],
                                               @"key2": @[QBVContainedIn(@[@"mogami", @"mikuma", @"suzuya", @"kumano"])],
                                               @"key3": @[QBVNot(QBVEqualTo(@(58)))],
                                               @"key4": @[QBVInRange(1, 100)],
                                               @"key5": @[QBVMatch(@"^[a-zA-Z0-9]+$")],
                                               @"key6": @[QBVEmail]
                                               }
                               errorMessages:&errorMessages];
    
    if (isValid) {
        NSLog(@"OK!");
    } else {
        NSLog(@"errorMessages: %@", errorMessages);
    }

Another way is combining with `UITextField`, explained below.


## Combine with forms
This library includes `UITextField+QBValidator` category that has the following methods.

* `- (void)setValidationRules:(NSArray *)rules`
* `- (NSArray *)validationRules`
* `- (BOOL)validate:(NSArray * __autoreleasing *)errorMessages`

So if you want to validate individual fields in your form, add rules by using `setValidationRules:` and validate by using `validateValue:name:rules:errorMessages:` of `QBValidator`.


## Create your own custom rules
You can create your own custom rules by subclassing `QBValidationRule`.  
The methods to be overridden are:

* Required
  * `- (BOOL)validateValue:(id)value`

* Optional
  * `- (NSString *)localizationTableName`
  * `- (NSString *)localizationKey`


## Localize
To localize error messages generated by `QBValidator`, you should create `QBValidator.strings` for different languages or create a subclass of `QBValidationRule` and override `localizationTableName` and `localizationKey`.


## Macros and functions
* `QBVRequired`
* `QBVEmail`
* `QBVURI`
* `QBVEqualTo(id value)`
* `QBVGreaterThan(id value)`
* `QBVGreaterThanOrEqualTo(id value)`
* `QBVLessThan(id value)`
* `QBVLessThanOrEqualTo(id value)`
* `QBVInRange(NSUInteger min, NSUInteger max)`
* `QBVInRange(NSUInteger min, NSUInteger max, BOOL inclusive)`
* `QBVContainedIn(NSArray *preferredValues)`
* `QBVNot(QBValidationRule *rule)`
* `QBVIf(QBValidationRule *rule, NSDictionary *conditionalRules)`
* `QBVMatch(NSString *pattern)`
* `QBVBlock(BOOL (^block)(id value))`
* `QBVKindOfClass(Class class)`
* `QBVMemberOfClass(Class class)`
* `QBVSubclassOfClass(Class class)`


## Installation
QBValidator can be installed via CocoaPods.

    pod 'QBValidator'

If you want to install manually, download this repository and copy files in QBValidator directory to your project.


## Tips
### Rule must be an array
**Note that rule must be an array.**  
For example:

    NSDictionary *rules = @{
    	@"key": @[QBVEqualTo(@"value")]
    };

**But you can omit array literals when there is only one rule.**  
For example:

    NSDictionary *rules = @{
    	@"key": QBVEqualTo(@"value") // This works.
    };
    
### Nesting Rules
If you want to validate complex data like `NSDictionary`, you can nest the rules.  
For example: (omitted array literals)

    [validator validateValues:@{
                                @"key1": @"value1",
                                @"key2": @{
                                    @"subkey1": @"subvalue1"
                                }
                        rules:@{
                                @"key1": QBVEqualTo(@"value1"),
                                @"key2": @{
                                    @"subkey1": QBVEqualTo(@"subvalue1")
                                }
                errorMessages:NULL];



## License
*QBValidator* is released under the **MIT License**, see *LICENSE.txt*.
