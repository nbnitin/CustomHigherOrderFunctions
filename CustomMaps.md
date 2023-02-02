**map(_:)**
Returns an array containing the results of mapping the given closure over the sequence's element

So technically map works on an array and transforms it into another array after performing some operation on the content of its elements. eg. It takes an array of strings and transforms it into an array of numbers by performing the count operation on the strings.

let cast = ["Vivien", "Marlon", "Kim", "Karl"]

let lowercaseNames = cast.map { $0.lowercased() }

// ‘lowercaseNames' == ["vivien", "marlon", "kim", "karl"]
let letterCounts = cast.map { $@.count }

// ‘letterCounts' == [6, 6, 3, 4]

Implementation:

We can have the extension of the array to get the Element and then we can apply a Transform which can be a closure defined as per client’s need. And that’s it.

extension Array {
    func myMap<Transform>(transform: (Element)-> Transform) -> [Transform] {
        var result = [Transform]()
        forEach { element in
            result.append(transform(element))
        }
        return result
    }
}

let a = [1,2,3,4]
print(a.myMap{$0*2}) // Usage

Compact Map:
Returns an array of non-nil results of calling the given transformation with each element of this sequence
Technically it does the same thing as map, but returns only non-nil elements.

Implementation:

So here we just require our transform to be non-nil. Easy peasy we can have a nil check on every transform and prevent it from adding on our array if it is nil.

extension Array {
    func myCompactMap<Transform>(transform: (Element)-> Transform?) -> [Transform] {
        var result = [Transform]()
        forEach { element in
            if let transformedEntity = transform(element) {
            result.append(transformedEntity)
            }
        }
        return result
    }
}

let a = ["1", "2", "shdh"]
print(a.myCompactMap{Int($0)})

Filter:
Returns an array containing, in order, the elements of sequence that statisfy the given predicate
Here our array should give only those elements which satisfy some condition.

Implementation:

Here we would need a closure that should return a Boolean and whatever element satisfies that condition should be returned.
extension Array {
    func myFilter(isIncluded: (Element)-> Bool) -> [Element] {
        var result = [Element]()
        forEach { element in
            if isIncluded(element) {
            result.append(element)
            }
        }
        return result
    }
}

let a = [1,2,3,4,5]
print(a.myFilter{$0 % 2 == 0})

Reduce:
Returns the results of combining the elements of the sequence using the given closure
So Reduce combines the elements of the array into a single value.

Implementation :

So this one is a little tricky. Let's think.
As we need to combine elements into one result we need a type for that. So we require a Generic here. Every time we use any combining algorithm we need some kind of variable for storing the combined value and it also provides us the initial value.
This would be our first parameter.
The second parameter is the function that combines the elements.

extension Array {
    func myReduce<Result>(_ initialResult: Result, _ nextPartialResult: (Result, Element)-> Result) -> Result {
         var result = initialResult
           forEach {
               result = nextPartialResult(result, $0)
           }
           return result
    }
}

let numbers = [1, 2, 3, 4]
let numberSum = numbers.myReduce(0, { sum, element in
    sum + element
})
print(numberSum) // prints 10

