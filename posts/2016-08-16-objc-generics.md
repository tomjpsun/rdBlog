Title: ObjC Generics
Date: 2016-08-16 11:00
Category: Objc
Tags: Generics
Has_Math: True


Apple support ObjC with "Generic" syntax since Xcode 7.
<!--More-->

###So, what's Generic...
In short, its like __template in C++__, you can specify object type in which the collection class (NSDictionary, NSArray, NSSet...) contains, then the compiler will check it for you.

The example in [Realm](https://realm.io/docs/objc/latest/) uses the Generic:

	// Dog model
	@interface Dog : RLMObject
		@property NSString *name;
		@property Person   *owner;
	@end
	RLM_ARRAY_TYPE(Dog) // define RLMArray<Dog> type

	// Person model
	@interface Person : RLMObject
		@property NSString             *name;
		@property NSDate               *birthdate;
		@property RLMArray<Dog *><Dog> *dogs;
		@end
	RLM_ARRAY_TYPE(Person) // define RLMArray<Person>

###Example explaination:

	RLMArray< Dog *>< Dog> * dogs

is "a pointer to RLMArray, which contains elements of < Dog * > type, and such type
conforms to the < Dog > protocol", here the second < Dog > is a protocol derived by macros in RLMObject.h:

	#define RLM_ARRAY_TYPE(RLM_OBJECT_SUBCLASS)\
	@protocol RLM_OBJECT_SUBCLASS <NSObject>   \
	@end

expands to:

	#define RLM_ARRAY_TYPE(Dog)\
	@protocol Dog <NSObject>   \
	@end

Conclusion: The generic syntax may let objC be closer to swift !
