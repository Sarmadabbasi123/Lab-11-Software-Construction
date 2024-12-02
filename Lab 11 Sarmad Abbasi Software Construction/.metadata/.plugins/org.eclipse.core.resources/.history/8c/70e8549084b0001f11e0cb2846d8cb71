package expressivo.expressions;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.Collection;
import java.util.Map;

import static expressivo.expressions.Numeric.*;
import static org.junit.Assert.assertEquals;

@RunWith(Parameterized.class)
public class ExpressionTest {

    private String value1;
    private String value2;

    // Constructor for Parameterized Tests
    public ExpressionTest(String value1, String value2) {
        this.value1 = value1;
        this.value2 = value2;
    }

    // Parameterized Data Provider
    @Parameterized.Parameters
    public static Collection<Object[]> numericTestData() {
        return Arrays.asList(new Object[][]{
                {"1", "1"},
                {"1.05", "1.05"},
                {"1.000", "1"}
        });
    }

    // ---------- NumericTest ----------
    @Test
    public void identicalNumericsRepresentedDifferentlyShouldBeEqual() {
        Numeric n1 = new Numeric(new BigDecimal(value1));
        Numeric n2 = new Numeric(new BigDecimal(value2));
        assertEquals(n1, n2);
        assertEquals(n1.toString(), n2.toString());
    }

    @Test
    public void numericsDifferentiateToZero() {
        Numeric numeric = new Numeric(5);
        Variable x = new Variable("x");
        assertEquals(ZERO, numeric.differentiate(x));
    }

    // ---------- EnvironmentParserTest ----------
    @Test
    public void convertsMapOfPrimitivesToMapOfExpressions() {
        Map<Variable, Numeric> expected = Map.of(
                new Variable("x"), ONE,
                new Variable("y"), TWO
        );

        Map<Variable, Numeric> actual = new EnvironmentParser(Map.of("x", 1., "y", 2.)).asExpressionMap();

        assertEquals(expected, actual);
    }

    // ---------- ComplexExpressionTest ----------
    @Test
    public void testComplexExpressions() {
        Variable w = new Variable("w");
        Variable x = new Variable("x");
        Variable y = new Variable("y");
        Variable z = new Variable("z");
        Numeric five = new Numeric(5);
        Numeric pi = new Numeric(BigDecimal.valueOf(3.1415927));

        assertEquals("(x+y)*5*(x+3.1415927)",
                new Product(new Product(new Sum(x, y), five), new Sum(x, pi)).toString());

        assertEquals("(x+y)*(5*(x+y)+3.1415927*(x+5))",
                new Product(new Sum(x, y),
                        new Sum(new Product(five, new Sum(x, y)), new Product(pi, new Sum(x, five)))).toString());

        Sum xPlusY = new Sum(x, y);
        Sum yPlusX = new Sum(y, x);
        Sum zPlusW = new Sum(z, w);
        Sum wPlusZ = new Sum(w, z);
        Sum xPlusYPlusZ = new Sum(new Sum(x, y), z);

        assertEquals("((x+y)*(x+y)+(z+w)*(z+w))*((w+z)*(w+z)+(y+x)*(y+x))",
                new Product(new Sum(pow(xPlusY, 2), pow(zPlusW, 2)),
                        new Sum(pow(wPlusZ, 2), pow(yPlusX, 2))).toString());

        assertEquals("(x+y+z)*(x+y+z)*(x+y+z)",
                pow(xPlusYPlusZ, 3).toString());
    }

    private Expression pow(Expression base, int exp) {
        Expression result = base;
        for (int i = 1; i < exp; i++) {
            result = new Product(result, base);
        }
        return result;
    }

    // ---------- ParenthesisedExpressionTest ----------
    @Test
    public void parensAreNotAddedWhenUnnecessary() {
        Variable a = new Variable("a");
        assertEquals("a*a+1", new Sum(new Product(a, a), ONE).toString());
    }

    @Test
    public void parensAreAddedToPreserveOperatorOrder() {
        Variable a = new Variable("a");
        assertEquals("(a+1)*a", new Product(new Sum(a, ONE), a).toString());
    }

    // ---------- VariableTest ----------
    @Test
    public void variableDifferentiatedByItselfIsOne() {
        Variable x = new Variable("x");
        assertEquals(ONE, x.differentiate(x));
    }

    @Test
    public void variableDifferentiatedByDifferentVariableIsZero() {
        Variable x = new Variable("x");
        Variable y = new Variable("y");
        assertEquals(ZERO, y.differentiate(x));
    }

    @Test
    public void variableIgnoresReplaceCallWithDifferentVariable() {
        Variable x = new Variable("x");
        Variable y = new Variable("y");
        assertEquals(x, x.replace(y, ONE));
    }

    @Test
    public void variableReplacesItselfWithNumeric() {
        Variable x = new Variable("x");
        assertEquals(ONE, x.replace(x, ONE));
    }

    // ---------- SumTest ----------
    @Test
    public void identicalSumsShouldBeEqual() {
        Sum sum = new Sum(ONE, new Variable("foo"));
        assertEquals(sum, sum);
    }

    @Test
    public void sumsReduceByAddingComponentNumerics() {
        assertEquals(TWO, new Sum(ONE, ONE).reduced());
    }
}
