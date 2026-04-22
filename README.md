## Rationale

The Casio scientific calculators like the fx880btg or the fx991ex are already capable of numerical integration for single integrals, but what if we want to calculate double integrals?

## Setup the template

To make this possible, we use Riemann sum to approximate the result, which can be implemented via the Spreadsheet functionality. In this section, we will setup a template so that we can just input bounds and the integral and get the result automatically.

Go to cell A3, press `TOOLS > Fill Formula`, set `Form = (A2-A1)/10`. This means A1 and A2 will hold the outer bounds, and we approximate with 10 slices, and A3 is the step size.

Go to cell B1, press `TOOLS > Fill Formula`, set `Form = A1+$A$3/2`. Then, go to cell B2, press `TOOLS > Fill Formula`, set `Form = B1+$A$3`, set `Range: B2:B10`. This generates 10 midpoints for the Riemann sum of the outer integral.

Go to cell D1, press `TOOLS > Fill Formula`, set `Form = SUM(C1:C10)*$A$3`. This calculates the Riemann sum, assuming C1 to C10 already had inner integral slices at different midpoints.

### Example

Now that we have this template, let's try with this integral:

$$\int_{0}^{1} \int_{x}^{x^2+1} (x + y) \, dy \, dx$$

First, insert 0 into A1, 1 into A2. Then, go to cell C1, press `TOOLS > Fill Formula`, set `Form = ∫(B1+x,B1,B1^2+1)`, set `Range: C1:C10`. Wait a bit, and the result will be in D1, which is `1.816`, pretty close to the actual result, which is `71/60`.

## Solving integrals with implicit bounds

But you might notice that this only works for explicit rectangle, type 1, and type 2 bounds. To extend further to implicit bounds, we need to use conditional expressions in Casio. From [nguyenphuminh/casio-coding](https://github.com/nguyenphuminh/casio-coding), we have the formulas:

* `Int((tanh(A-B)+2)/2)` would return 1 if `A >= B`, 0 otherwise.
* `Int(1-|tanh(A-B)|)` would return 1 if `A = B`, 0 otherwise.

Example:

$$\iint_{D} x + y \, dx \, dy, \quad \text{where } D: \begin{cases} x, y \geq 0 \\ x + y \leq 1 \end{cases}$$

We know that x and y can only be between 0 and 1, so first the outer bounds are 0 and 1, then, we structure the inner integral formula like this to ensure the Riemann sum only includes satisfying slices:

```
Form = ∫((B1+x)*Int((tanh(1-x-y)+2)/2),0,1)
```

Although, do note that these conditional expressions are extremely costly/slow so always resolve your bounds first if possible.

## Also check out

* [casio-coding](https://github.com/nguyenphuminh/casio-coding): Programming on Casio "non-programmable" scientific calculators using built-in functions.
* [casio-machine-learning](https://github.com/nguyenphuminh/casio-machine-learning): Machine learning on Casio scientific calculators.
