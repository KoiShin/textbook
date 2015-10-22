
課題
----

分数の四則演算を楽に行うために、Rational（分数）クラスの作成をせよ。
Rationalクラスは、分子と分母をフィールドとして持っている。

Rationalクラスができること：

- 初期化の時
	- `new Rational(2, 3)` → 分数 2/3 を表す
	- `new Rational(4, 8)` → 分数 1/2
	- `new Rational(3)` → 分数 3/1
- 四則演算
	- 分数同士の足し算（plus）引き算（minus）掛け算（times）割り算（divide）を実装する
	- 以下の例：num1 = 2/3, num2 = 5,6 とするとき、
	- `num1.plus(num2)` → 分数 3/2
	- `num1.minus(num2)` → 分数 -1/6
	- `num1.times(num2)` → 分数 5/9
	- `num1.divide(num2)` → 分数 4/5
- 変換
	- `(new Rational(1, 3)).toString()` → 文字列 "(1/3)" を返す

動作例
------

Main.java

~~~ java
Rational num1 = new Rational(2, 3);
Rational num2 = new Rational(5, 6);

System.out.println(num1.plus(num2));   // -> (3/2)
System.out.println(num1.minus(num2));  // -> (-1/6)
System.out.println(num1.times(num2));  // -> (5/9)
System.out.println(num1.divide(num2)); // -> (4/5)

System.out.println(new Rational(3));   // -> (3/1)
~~~

|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  
|  


回答
-----

Rational.java

~~~ java
package rational;

public class Rational {
	// rational: (numerator/denominator)
	private int numerator;
	private int denominator;

	// create a new Rational object with one argument
	public Rational(int integer) {
		this(integer, 1);
	}

	// create a new Rational object with two argument
	public Rational(int numerator, int denominator) {
		if (denominator == 0) {
			throw new RuntimeException("Denominator is zero");
		}
		// reduce
		int common = gcd(numerator, denominator);
		this.numerator   = numerator   / common;
		this.denominator = denominator / common;

		// move minus sign: (a/-b) to (-a/b)
		if (this.denominator < 0) {
			this.numerator   *= -1;
			this.denominator *= -1;
		}
	}

	public String toString() {
		return "(" + numerator + "/" + denominator + ")";
	}

	// return (this + other)
	public Rational plus(Rational other) {
		return new Rational(
			(this.numerator * other.denominator) + (this.denominator * other.numerator),
			this.denominator * other.denominator
		);
	}

	// return (this - other)
	public Rational minus(Rational other) {
		return this.plus(
			new Rational(-other.numerator, other.denominator)
		);
	}

	// return (this * other)
	public Rational times(Rational other) {
		return new Rational(
			this.numerator * other.numerator,
			this.denominator * other.denominator
		);
	}

	// return (this / other)
	public Rational divide(Rational other) {
		return this.times(other.reciprocal());
	}

	// return (1 / this)
	public Rational reciprocal() {
		return new Rational(denominator, numerator);
	}

	// return gcd(m, n)
	private static int gcd(int m, int n) {
		if (0 == n) return m;
		else return gcd(n, m % n);
	}
}
~~~













