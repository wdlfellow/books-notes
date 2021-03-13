# Java8 in action

## lambda


## stream

- stream();
- filter();
- map();
- flatMap() --combine all the streams into one

- district();
- count();
- limit();
- skip();

- findAny(), findFirst()
- anyMatch(), allMatch(), noneMatch()
- Arrays.stream()

## Stateless & Stateful
- stateless: filter(), map()
- stateful: sum(), count(), sort()

## Collect
- menu.stream().collect(groupingBy(Dish::getType))


## Code example
- Optional<Integer> max = numbers.stream().reduce(Integer::max)
- Stream<String> lines = Files.lines(Paths.get("data.txt"), Charset.defaultCharset())

