## Random

랜덤값을 구할 때 사용되는 클래스 

```java
int randomInt = random.nextInt();
System.out.println("randomInt = " + randomInt);

double randomDouble = random.nextDouble();//0.0d ~ 1.0d
System.out.println("randomDouble = " + randomDouble);

boolean randomBoolean = random.nextBoolean();
System.out.println("randomBoolean = " + randomBoolean);

// 범위 조회
int randomRange1 = random.nextInt(10);// 0 ~ 9 출력
System.out.println("0 ~ 9 = " + randomRange1);
int randomRange2 = random.nextInt(10) + 1; // 1 ~ 10까지 출력
System.out.println("0 ~ 10 = " + randomRange2);
```



### Seed

Random 클래스는 내부에서 Seed값을 사용해 랜덤 값을 구함

Seed 값이 같으면 항상 같은 결과가 출력됨

```java
// 기본 생성자의 경우 seedUniQuifier, nanoTime을 조합해 Seed를 만든다.
public Random() {
        this(seedUniquifier() ^ System.nanoTime());
    }

    private static long seedUniquifier() {
        // L'Ecuyer, "Tables of Linear Congruential Generators of
        // Different Sizes and Good Lattice Structure", 1999
        for (;;) {
            long current = seedUniquifier.get();
            long next = current * 1181783497276652981L;
            if (seedUniquifier.compareAndSet(current, next))
                return next;
        }
    }

    private static final AtomicLong seedUniquifier
            = new AtomicLong(8682522807148012L);

// 
public Random(long seed) {
    if (getClass() == Random.class)
        this.seed = new AtomicLong(initialScramble(seed));
    else {
        // subclass might have overridden setSeed
        this.seed = new AtomicLong();
        setSeed(seed);
    }
}
```

