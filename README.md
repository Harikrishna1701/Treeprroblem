import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

public class RestaurantFinder2 {

    public static void main(String[] args) {
        String filePath = "C:\\Users\\Sahith\\OneDrive\\Documents\\data.csv"; // Correct path
        System.out.println("Current working directory: " + System.getProperty("user.dir"));
        System.out.println("Attempting to read from: " + filePath);

        // Check if the file exists
        File file = new File(filePath);
        if (!file.exists()) {
            System.out.println("File does not exist!");
            return; // Exit if file is not found
        }

        // Example food items to search (modify this array to test different inputs)
        String[][] foodItemsToSearch = {
            {"burger", "tofu_log"}, // Input 1
            {"chef_salad", "wine_spritzer"}, // Input 2
            {"extreme_fajita", "jalapeno_poppers", "extra_salsa"} // Input 3
        };

        // Loop through each set of food items and find the minimum price
        for (String[] items : foodItemsToSearch) {
            String result = findMinimumPrice(filePath, items);
            System.out.println("Input: " + String.join(", ", items) + " -> " + result);
        }
    }

    public static String findMinimumPrice(String filePath, String[] foodItemNames) {
        // Create a map to store restaurant data
        Map<Integer, Map<String, Double>> restaurantMap = new HashMap<>();

        // Read the data from the CSV file
        readDataFromFile(filePath, restaurantMap);

        // Initialize variables to track the minimum price
        double minPrice = Double.MAX_VALUE;
        int restaurantId = -1;

        // Loop through each restaurant to find the minimum price for the specified food items
        for (Map.Entry<Integer, Map<String, Double>> entry : restaurantMap.entrySet()) {
            int id = entry.getKey();
            Map<String, Double> foodItems = entry.getValue();

            // Check if all food items are available in the restaurant
            boolean allItemsAvailable = true;
            double totalPrice = 0.0;

            for (String foodItem : foodItemNames) {
                if (foodItems.containsKey(foodItem)) {
                    totalPrice += foodItems.get(foodItem);
                } else {
                    allItemsAvailable = false;
                    break; // Exit if any item is not available
                }
            }

            // Update the minimum price if all items are available
            if (allItemsAvailable && totalPrice < minPrice) {
                minPrice = totalPrice;
                restaurantId = id;
            }
        }

        // Return the result
        if (restaurantId == -1) {
            return "No matching restaurant found";
        } else {
            return restaurantId + ", " + String.format("%.2f", minPrice);
        }
    }

    public static void readDataFromFile(String filePath, Map<Integer, Map<String, Double>> restaurantMap) {
        String line = null; // Declare line outside the try block
        try (BufferedReader br = new BufferedReader(new FileReader(filePath))) {
            while ((line = br.readLine()) != null) {
                String[] data = line.split(",");
                if (data.length != 3) {
                    continue; // Skip invalid lines
                }
                int restaurantId = Integer.parseInt(data[0].trim());
                double price = Double.parseDouble(data[1].trim());
                String foodItemName = data[2].trim();

                // Add to the restaurant map
                restaurantMap
                    .computeIfAbsent(restaurantId, k -> new HashMap<>())
                    .put(foodItemName, price);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } catch (NumberFormatException e) {
            System.out.println("Skipping invalid line: " + line); // Now accessible
        }
    }
}
