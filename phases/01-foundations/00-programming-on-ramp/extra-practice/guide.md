# Extra practice for the on-ramp: 15 exercises with answers

← [Week 0: programming on-ramp](../README.md) · [Phase 1](../../README.md) · [Home](../../../../README.md)

More practice at the same level as lessons 0.1 to 0.3, for anyone who scored under 12 on the entry check, or who finished a lesson and doesn't yet feel sure. Five exercises per lesson, in banking examples. **Try each one before looking at its answer** at the bottom of the page: being stuck for a while is how the learning happens. Every answer was run with Java 21, and the outputs shown are real.

Money is in **kobo** in a `long`, as in the lessons: ₦250.75 is `25075`.

## After lesson 0.1: values, variables, input and errors

1. **A receipt.** Print three lines: `ITLearning Bank`, your full name, and `Account type: Savings`.
2. **A deposit.** Start with a balance of ₦15,000.00 in kobo. Add a deposit of ₦2,500.50. Print the new balance in kobo, then as naira with two decimal places, using `/ 100` and `% 100`. Expected: `New balance in kobo: 1750050`, then `New balance: ₦17500.50`.
3. **Input.** Ask for the user's first name and age, then print `Welcome, Ada. In 5 years you will be 27.` (with their name and age).
4. **Find and fix the errors.** This program has three mistakes. Run it, read the first error, fix it, and run it again until it works:

   ```java
   public class Ex04 {
       public static void main(String[] args) {
           String age = 25;
           long savingsKobo = 900_000
           int monthly = savingsKobo / 12;
           System.out.println("Age: " + age);
           System.out.println("Monthly saving in kobo: " + monthly);
       }
   }
   ```

5. **Sharing a bill.** Split ₦10,000.00 equally between 3 people. Print each share in kobo and what's left over, then check that `share × people + left over` equals the total.

## After lesson 0.2: decisions, loops, arrays and strings

6. **Transfer fees.** The fee is ₦10 for amounts up to ₦5,000, ₦25 up to ₦50,000, and ₦50 above that. Print the fee for 300000, 500000, 500001, 5000000 and 7500000 kobo. Check the boundaries: 500000 and 500001 must get different fees.
7. **A savings plan.** Save ₦1,000 every month for 12 months. Print the total only at the end of every third month (months 3, 6, 9 and 12).
8. **A week of sales.** A market stall's daily sales in kobo are `{1_200_000, 0, 850_000, 1_500_000, 0, 400_000, 980_000}`. Print the total, the best day, and how many days had no sales.
9. **Masking.** Print the account number `0123456789` with all but the last four digits replaced by `*`. Then count the digits in `"Pay 2500 to 0123456789"`.
10. **Until zero.** Keep reading amounts in kobo until the user types `0`, then print how many amounts were entered and their total.

## After lesson 0.3: methods

11. **`isValidPin`.** Write `static boolean isValidPin(String pin)`: true only for exactly four digits. Test it with `"1234"`, `"123"` and `"12a4"`.
12. **`feeFor`.** Turn exercise 6 into `static long feeFor(long amountKobo)`, and call it from a loop.
13. **`formatNaira`.** Write `static String formatNaira(long kobo)` that returns `₦1,250,000.50` for `125_000_050`, with commas between thousands.
14. **`canTransfer`.** Write `static boolean canTransfer(long balanceKobo, long amountKobo, boolean frozen)`: true only if the wallet isn't frozen, the amount is positive and the balance covers it.
15. **Tidy up.** Take your longest program from this page and split it into at least three methods with clear names. There's no single right answer: compare with a classmate in the forum.

## Answers

Type them out rather than copying: your fingers learn too.

**1.**

```java
public class Ex01 {
    public static void main(String[] args) {
        System.out.println("ITLearning Bank");
        System.out.println("Ada Obi");
        System.out.println("Account type: Savings");
    }
}
```

**2.**

```java
public class Ex02 {
    public static void main(String[] args) {
        long balanceKobo = 1_500_000;          // ₦15,000.00
        long depositKobo = 250_050;            // ₦2,500.50
        balanceKobo = balanceKobo + depositKobo;
        System.out.println("New balance in kobo: " + balanceKobo);
        System.out.println("New balance: ₦" + balanceKobo / 100 + "." + String.format("%02d", balanceKobo % 100));
    }
}
```

`String.format("%02d", ...)` prints the kobo with two digits, so 5 kobo shows as `05`, not `5`.

**3.**

```java
import java.util.Scanner;

public class Ex03 {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        System.out.print("Your first name: ");
        String name = in.nextLine();
        System.out.print("Your age: ");
        int age = in.nextInt();
        System.out.println("Welcome, " + name + ". In 5 years you will be " + (age + 5) + ".");
    }
}
```

The brackets in `(age + 5)` matter: without them Java joins the text and the numbers, and prints `225` for an age of 22.

**4.** Java reports one error first:

```
Ex04.java:4: error: ';' expected
        long savingsKobo = 900_000
                                  ^
```

After you add the semicolon, it finds the other two:

```
Ex04.java:3: error: incompatible types: int cannot be converted to String
        String age = 25;
                     ^
Ex04.java:5: error: incompatible types: possible lossy conversion from long to int
        int monthly = savingsKobo / 12;
                                  ^
```

The lesson in that: fix the **first** error, then run again. A missing semicolon can hide every other mistake. The fixed program:

```java
public class Ex04 {
    public static void main(String[] args) {
        int age = 25;                          // a number, not text
        long savingsKobo = 900_000;            // the missing semicolon
        long monthly = savingsKobo / 12;       // long divided by a number is still a long
        System.out.println("Age: " + age);
        System.out.println("Monthly saving in kobo: " + monthly);
    }
}
```

It prints `Monthly saving in kobo: 75000`.

**5.**

```java
public class Ex05 {
    public static void main(String[] args) {
        long totalKobo = 1_000_000;   // ₦10,000.00
        int people = 3;
        long share = totalKobo / people;
        long left = totalKobo % people;
        System.out.println("Each person gets " + share + " kobo");
        System.out.println("Left over: " + left + " kobo");
        System.out.println("Check: " + (share * people + left == totalKobo));
    }
}
```

It prints `333333` kobo each, `1` kobo left over, and `Check: true`. Banks never lose that last kobo: they decide who gets it.

**6.**

```java
public class Ex06 {
    public static void main(String[] args) {
        long[] amounts = {300_000, 500_000, 500_001, 5_000_000, 7_500_000};
        for (long amount : amounts) {
            long fee;
            if (amount <= 500_000) {
                fee = 1_000;
            } else if (amount <= 5_000_000) {
                fee = 2_500;
            } else {
                fee = 5_000;
            }
            System.out.println(amount + " kobo -> fee " + fee + " kobo");
        }
    }
}
```

The fees are 1000, 1000, 2500, 2500 and 5000 kobo. `<=` puts exactly ₦5,000 in the lower band; `<` would have put it in the higher one.

**7.**

```java
public class Ex07 {
    public static void main(String[] args) {
        long totalKobo = 0;
        for (int month = 1; month <= 12; month++) {
            totalKobo = totalKobo + 100_000;          // ₦1,000.00 a month
            if (month % 3 == 0) {
                System.out.println("Month " + month + ": " + totalKobo + " kobo");
            }
        }
    }
}
```

It prints 300000, 600000, 900000 and 1200000 kobo for months 3, 6, 9 and 12. `month % 3 == 0` is true when the month divides by 3 exactly.

**8.**

```java
public class Ex08 {
    public static void main(String[] args) {
        long[] salesKobo = {1_200_000, 0, 850_000, 1_500_000, 0, 400_000, 980_000};
        long total = 0;
        long best = salesKobo[0];
        int zeroDays = 0;
        for (long s : salesKobo) {
            total += s;
            if (s > best) {
                best = s;
            }
            if (s == 0) {
                zeroDays++;
            }
        }
        System.out.println("Total: " + total + " kobo");
        System.out.println("Best day: " + best + " kobo");
        System.out.println("Days with no sales: " + zeroDays);
    }
}
```

Total 4930000 kobo, best day 1500000 kobo, 2 days with no sales. Starting `best` at the first day's value, not at 0, keeps it right even if every day were negative (a refund day).

**9.**

```java
public class Ex09 {
    public static void main(String[] args) {
        String account = "0123456789";
        String masked = "*".repeat(account.length() - 4) + account.substring(account.length() - 4);
        System.out.println(masked);
        String input = "Pay 2500 to 0123456789";
        int digits = 0;
        for (int i = 0; i < input.length(); i++) {
            if (Character.isDigit(input.charAt(i))) {
                digits++;
            }
        }
        System.out.println("Digits: " + digits);
    }
}
```

It prints `******6789` and `Digits: 14`.

**10.**

```java
import java.util.Scanner;

public class Ex10 {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        long total = 0;
        int count = 0;
        System.out.print("Amount in kobo (0 to finish): ");
        long amount = in.nextLong();
        while (amount != 0) {
            total += amount;
            count++;
            System.out.print("Amount in kobo (0 to finish): ");
            amount = in.nextLong();
        }
        System.out.println(count + " amounts, total " + total + " kobo");
    }
}
```

Typing 250000, 100000, 50 and 0 prints `3 amounts, total 350050 kobo`. The first amount is read before the loop, so typing 0 straight away prints `0 amounts`.

**11 to 14**, together:

```java
import java.util.Locale;

public class Ex11to14 {
    static boolean isValidPin(String pin) {
        if (pin.length() != 4) {
            return false;
        }
        for (int i = 0; i < pin.length(); i++) {
            if (!Character.isDigit(pin.charAt(i))) {
                return false;
            }
        }
        return true;
    }

    static long feeFor(long amountKobo) {
        if (amountKobo <= 500_000) {
            return 1_000;
        } else if (amountKobo <= 5_000_000) {
            return 2_500;
        }
        return 5_000;
    }

    static String formatNaira(long kobo) {
        return String.format(Locale.UK, "₦%,d.%02d", kobo / 100, kobo % 100);
    }

    static boolean canTransfer(long balanceKobo, long amountKobo, boolean frozen) {
        return !frozen && amountKobo > 0 && balanceKobo >= amountKobo;
    }

    public static void main(String[] args) {
        System.out.println(isValidPin("1234") + " " + isValidPin("123") + " " + isValidPin("12a4"));
        for (long amount : new long[] {450_000, 2_000_000, 6_000_000}) {
            System.out.println(formatNaira(amount) + " -> fee " + formatNaira(feeFor(amount)));
        }
        System.out.println(formatNaira(125_000_050));
        System.out.println(canTransfer(500_000, 200_000, false) + " " + canTransfer(500_000, 600_000, false) + " "
                + canTransfer(500_000, 200_000, true));
    }
}
```

Output:

```
true false false
₦4,500.00 -> fee ₦10.00
₦20,000.00 -> fee ₦25.00
₦60,000.00 -> fee ₦50.00
₦1,250,000.50
true false false
```

`%,d` adds the thousands commas; `Locale.UK` makes sure they're commas on every computer, whatever its language settings.

**15.** Any split where each method does one job and its name says what: for example `readAmounts`, `totalOf` and `printSummary` for exercise 10. Post yours in the *Questions and help* forum and compare.
