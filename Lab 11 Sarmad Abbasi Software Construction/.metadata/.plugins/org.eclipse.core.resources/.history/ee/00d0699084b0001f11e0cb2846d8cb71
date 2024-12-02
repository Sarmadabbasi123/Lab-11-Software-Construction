package expressivo;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;

import java.util.Arrays;
import java.util.Collection;
import java.util.Map;

import static org.junit.Assert.assertEquals;

public class CommandsTest {

    @RunWith(Parameterized.class)
    public static class DifferentiateTest {
        private final String inputExpr;
        private final String variable;
        private final String expectedOutput;

        public DifferentiateTest(String inputExpr, String variable, String expectedOutput) {
            this.inputExpr = inputExpr;
            this.variable = variable;
            this.expectedOutput = expectedOutput;
        }

        @Parameterized.Parameters
        public static Collection<Object[]> data() {
            return Arrays.asList(new Object[][]{
                    {"1", "x", "0"},
                    {"c", "x", "0"},
                    {"x", "x", "1"},
                    {"1+1", "x", "0"},
                    {"x+2", "x", "1"},
                    {"x+x", "x", "2"}
            });
        }

        @Test
        public void testDifferentiate() {
            assertEquals(expectedOutput, Commands.differentiate(inputExpr, variable));
        }
    }

    @RunWith(Parameterized.class)
    public static class SimplifyTest {
        private final String input;
        private final Map<String, Double> environment;
        private final String expectedResult;

        public SimplifyTest(String input, Map<String, Double> environment, String expectedResult) {
            this.input = input;
            this.environment = environment;
            this.expectedResult = expectedResult;
        }

        @Parameterized.Parameters
        public static Collection<Object[]> data() {
            return Arrays.asList(new Object[][]{
                    {"1", Map.of("x", 5.), "1"},
                    {"y", Map.of("x", 5.), "y"},
                    {"x", Map.of("x", 5.), "5"},
                    {"x+y+z", Map.of("x", 1., "y", 2.), "1+2+z"},
                    {"t+u+v+w+x+y+z", Map.of("x", 1., "y", 2.), "t+u+v+w+1+2+z"},
                    {"x*y*z", Map.of("x", 5., "y", 10.5), "5*10.5*z"},
                    {"t*u*v*w*x*y*z", Map.of("x", 5., "y", 10.5), "t*u*v*w*5*10.5*z"},
                    {"x+1", Map.of("x", 1.), "2"}
            });
        }

        @Test
        public void testSimplify() {
            assertEquals(expectedResult, Commands.simplify(input, environment));
        }
    }
}
