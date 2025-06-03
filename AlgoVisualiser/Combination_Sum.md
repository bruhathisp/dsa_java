``` java
// import visualization libraries {
import org.algorithm_visualizer.*;
import java.util.*;
// }

public class Main {
    // define tracer variables {
    Array1DTracer inputTracer = new Array1DTracer("Candidates");
    LogTracer logTracer = new LogTracer("Execution Log");
    // }

    int[] candidates = {2, 3, 7};
    List<List<Integer>> result = new ArrayList<>();

    void backtrack(int target, int start, List<Integer> current) {
    logTracer.println("Target: " + target + ", Start index: " + start + ", Current list: " + current);
    Tracer.delay();

    if (target == 0) {
        result.add(new ArrayList<>(current));
        logTracer.println("✅ Found combination: " + current);
        Tracer.delay();
        return;
    }

    for (int i = start; i < candidates.length; i++) {
        logTracer.println("🔁 Loop i = " + i + ", candidates[" + i + "] = " + candidates[i]);

        if (candidates[i] > target) {
            logTracer.println("⛔ Skip candidates[" + i + "] = " + candidates[i] + " (too large)");
            Tracer.delay();
            continue;
        }

        current.add(candidates[i]);
        inputTracer.select(i);
        logTracer.println("➡️ Include candidates[" + i + "] = " + candidates[i] + ", move deeper with new target = " + (target - candidates[i]));
        Tracer.delay();

        backtrack(target - candidates[i], i, current);

        int removed = current.remove(current.size() - 1);
        inputTracer.deselect(i);
        logTracer.println("⬅️ Backtrack: Removed " + removed + ", back to i = " + i + ", current: " + current);
        Tracer.delay();
    }

    logTracer.println("🔚 End of loop for start = " + start);
    Tracer.delay();
}


    Main() {
        // layout setup {
        Layout.setRoot(new VerticalLayout(new Commander[]{inputTracer, logTracer}));
        inputTracer.set(candidates);
        logTracer.println("Input Candidates: " + Arrays.toString(candidates));
        Tracer.delay();
        // }

        logTracer.println("Starting Combination Sum Backtracking");
        backtrack(7, 0, new ArrayList<>());
        logTracer.println("All combinations that sum to target: " + result);
    }

    public static void main(String[] args) {
        new Main();
    }
}

```
