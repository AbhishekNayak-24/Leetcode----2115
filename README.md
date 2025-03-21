# Leetcode----2115
Find All Possible Recipes From Given Recipes
//code in java
import java.util.*;

public class Solution {
    public List<String> findAllRecipes(String[] recipes, List<List<String>> ingredients, String[] supplies) {
        Map<String, List<String>> graph = new HashMap<>();
        Map<String, Integer> inDegree = new HashMap<>();
        Set<String> available = new HashSet<>(Arrays.asList(supplies));

        // Build graph and initialize in-degree
        for (int i = 0; i < recipes.length; i++) {
            String recipe = recipes[i];
            for (String ingredient : ingredients.get(i)) {
                graph.putIfAbsent(ingredient, new ArrayList<>());
                graph.get(ingredient).add(recipe);
                inDegree.put(recipe, inDegree.getOrDefault(recipe, 0) + 1);
            }
        }

        // Add recipes with no dependencies to in-degree map
        for (String recipe : recipes) {
            inDegree.putIfAbsent(recipe, 0);
        }

        // Perform topological sort
        Queue<String> queue = new LinkedList<>();
        for (String supply : supplies) {
            queue.offer(supply);
        }

        List<String> result = new ArrayList<>();
        while (!queue.isEmpty()) {
            String current = queue.poll();
            if (recipes != null && Arrays.asList(recipes).contains(current)) {
                result.add(current);
            }
            for (String next : graph.getOrDefault(current, new ArrayList<>())) {
                inDegree.put(next, inDegree.get(next) - 1);
                if (inDegree.get(next) == 0) {
                    queue.offer(next);
                }
            }
        }

        return result;
    }
}
