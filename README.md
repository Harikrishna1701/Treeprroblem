# Treeprroblem
import java.util.HashMap;
import java.util.Map;

public class FileSystem {
    // The filesystem representation
    static Map<String, Object> filesystem = new HashMap<>();

    static {
        filesystem.put("root", new HashMap<String, Object>() {{
            put("dir1", new HashMap<String, Object>() {{
                put("subdir1", new HashMap<String, Object>() {{
                    put("file1.txt", 100);
                    put("file2.txt", 200);
                    put("subsubdir1", new HashMap<String, Object>() {{
                        put("file3.txt", 50);
                        put("file4.txt", 150);
                        put("subsubsubdir1", new HashMap<String, Object>() {{
                            put("file5.txt", 500);
                            put("emptydir1", new HashMap<String, Object>());
                        }});
                    }});
                }});
                put("subdir2", new HashMap<String, Object>() {{
                    put("file6.txt", 300);
                    put("subsubdir2", new HashMap<String, Object>() {{
                        put("file7.txt", 700);
                        put("subsubsubdir2", new HashMap<String, Object>() {{
                            put("file8.txt", 800);
                            put("file9.txt", 900);
                        }});
                    }});
                    put("emptydir2", new HashMap<String, Object>());
                }});
                put("file10.txt", 1000);
            }});
            put("dir2", new HashMap<String, Object>() {{
                put("subdir3", new HashMap<String, Object>() {{
                    put("subsubdir3", new HashMap<String, Object>() {{
                        put("file11.txt", 400);
                        put("file12.txt", 500);
                    }});
                    put("subsubdir4", new HashMap<String, Object>() {{
                        put("emptydir3", new HashMap<String, Object>());
                    }});
                }});
                put("subdir4", new HashMap<String, Object>() {{
                    put("file13.txt", 600);
                    put("subsubdir5", new HashMap<String, Object>() {{
                        put("file14.txt", 700);
                        put("file15.txt", 800);
                        put("subsubsubdir3", new HashMap<String, Object>() {{
                            put("emptydir4", new HashMap<String, Object>());
                            put("file16.txt", 900);
                            put("file17.txt", 1000);
                        }});
                    }});
                }});
                put("emptydir5", new HashMap<String, Object>());
            }});
            put("dir3", new HashMap<String, Object>() {{
                put("file18.txt", 1100);
                put("subdir5", new HashMap<String, Object>() {{
                    put("subsubdir6", new HashMap<String, Object>() {{
                        put("file19.txt", 1200);
                        put("subsubsubdir4", new HashMap<String, Object>() {{
                            put("file20.txt", 1300);
                            put("emptydir6", new HashMap<String, Object>());
                        }});
                    }});
                }});
            }});
            put("emptydir7", new HashMap<String, Object>());
            put("file21.txt", 1400);
        }});
    }

    public static void main(String[] args) {
        System.out.println(calculateTotalSize("root.dir1.subdir1")); // Expected Output: Total size: 1000
        System.out.println(calculateTotalSize("root.dir2.subdir4.subsubdir5")); // Expected Output: Total size: 3400
    }

    public static String calculateTotalSize(String path) {
        String[] components = path.split("\\.");
        Object currentDir = filesystem;

        // Traverse the filesystem to reach the specified directory
        for (String component : components) {
            if (currentDir instanceof Map<?, ?> && ((Map<?, ?>) currentDir).containsKey(component)) {
                currentDir = ((Map<?, ?>) currentDir).get(component);
            } else {
                return "Directory '" + path + "' not found.";
            }
        }

        // Calculate total size of files in the specified directory
        int totalSize = getTotalSize(currentDir);
        return "Total size: " + totalSize;
    }

    private static int getTotalSize(Object directory) {
        if (!(directory instanceof Map<?, ?>)) {
            return 0;
        }

        int totalSize = 0;
        for (Map.Entry<?, ?> entry : ((Map<?, ?>) directory).entrySet()) {
            if (entry.getValue() instanceof Integer) { // It's a file
                totalSize += (Integer) entry.getValue();
            } else { // It's a directory
                totalSize += getTotalSize(entry.getValue());
            }
        }
        return totalSize;
    }
}
