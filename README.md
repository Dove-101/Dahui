import java.io.*;

public class ConvertEncoding {
    public static void main(String[] args) {
        try {
            File inputFile = new File("ktt.txt");
            File outputFile = new File("ktt_gbk.txt");

            BufferedReader reader = new BufferedReader(new InputStreamReader(new FileInputStream(inputFile), "UTF-8"));
            BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(new FileOutputStream(outputFile), "GBK"));

            String line;
            while((line = reader.readLine()) != null) {
                writer.write(line);
                writer.newLine();
            }

            reader.close();
            writer.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

//TIP 要<b>运行</b>代码，请按 <shortcut actionId="Run"/> 或
// 点击间距中的 <icon src="AllIcons.Actions.Execute"/> 图标。
public class Main {
    public static void main(String[] args) {
        //TIP 当文本光标位于高亮显示的文本处时按 <shortcut actionId="ShowIntentionActions"/>
        // 查看 IntelliJ IDEA 建议如何修复。
        System.out.printf("Hello and welcome!");

        for (int i = 1; i <= 5; i++) {
            //TIP 按 <shortcut actionId="Debug"/> 开始调试代码。我们已经设置了一个 <icon src="AllIcons.Debugger.Db_set_breakpoint"/> 断点
            // 但您始终可以通过按 <shortcut actionId="ToggleLineBreakpoint"/> 添加更多断点。
            System.out.println("i = " + i);
        }
    }
}
import java.util.Scanner;

public class MainMenu {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ProductManager productManager = new ProductManager();
        SalesStatistics salesStatistics = new SalesStatistics(productManager);

        while (true) {
            System.out.println("=== 主菜单 ===");
            System.out.println("1. 商品基本信息管理系统");
            System.out.println("2. 商品销售统计");
            System.out.println("3. 退出");
            System.out.print("请选择功能：");
            int choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    productManagementMenu(scanner, productManager);
                    break;
                case 2:
                    salesStatisticsMenu(scanner, salesStatistics);
                    break;
                case 3:
                    System.out.println("感谢使用，再见！");
                    scanner.close();
                    System.exit(0);
                default:
                    System.out.println("无效的选项，请重新选择！");
            }
        }
    }

    public static void productManagementMenu(Scanner scanner, ProductManager productManager) {
        while (true) {
            System.out.println("\n=== 商品基本信息管理系统 ===");
            System.out.println("1. 添加商品信息");
            System.out.println("2. 删除商品信息");
            System.out.println("3. 修改商品信息");
            System.out.println("4. 显示商品信息");
            System.out.println("5. 导入商品信息");
            System.out.println("6. 导出商品信息");
            System.out.println("7. 返回主菜单");
            System.out.print("请选择功能：");
            int choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    productManager.addProduct(scanner);
                    break;
                case 2:
                    productManager.deleteProduct(scanner);
                    break;
                case 3:
                    productManager.modifyProduct(scanner);
                    break;
                case 4:
                    productManager.displayProducts();
                    break;
                case 5:
                    productManager.importProducts(scanner);
                    break;
                case 6:
                    productManager.exportProducts();
                    break;
                case 7:
                    return;
                default:
                    System.out.println("无效的选项，请重新选择！");
            }
        }
    }

    public static void salesStatisticsMenu(Scanner scanner, SalesStatistics salesStatistics) {
        while (true) {
            System.out.println("\n=== 商品销售统计 ===");
            System.out.println("1. 按季度统计商品的最高销量");
            System.out.println("2. 按季度统计商品的最低销量");
            System.out.println("3. 按季度统计商品的平均销量");
            System.out.println("4. 返回主菜单");
            System.out.print("请选择功能：");
            int choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    salesStatistics.highestSalesByQuarter();
                    break;
                case 2:
                    salesStatistics.lowestSalesByQuarter();
                    break;
                case 3:
                    salesStatistics.averageSalesByQuarter();
                    break;
                case 4:
                    return;
                default:
                    System.out.println("无效的选项，请重新选择！");
            }
        }
    }
}
public class Product {
    private String id;
    private String name;
    private int q1Sales;
    private int q2Sales;
    private int q3Sales;

    public Product(String id, String name, int q1Sales, int q2Sales, int q3Sales) {
        this.id = id;
        this.name = name;
        this.q1Sales = q1Sales;
        this.q2Sales = q2Sales;
        this.q3Sales = q3Sales;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getQ1Sales() {
        return q1Sales;
    }

    public void setQ1Sales(int q1Sales) {
        this.q1Sales = q1Sales;
    }

    public int getQ2Sales() {
        return q2Sales;
    }

    public void setQ2Sales(int q2Sales) {
        this.q2Sales = q2Sales;
    }

    public int getQ3Sales() {
        return q3Sales;
    }

    public void setQ3Sales(int q3Sales) {
        this.q3Sales = q3Sales;
    }

    public int getTotalSales() {
        return q1Sales + q2Sales + q3Sales;
    }

    public double getAverageSales() {
        return (q1Sales + q2Sales + q3Sales) / 3.0;
    }

    @Override
    public String toString() {
        return "Product{id='" + id + "', name='" + name + "', Q1 Sales=" + q1Sales + ", Q2 Sales=" + q2Sales + ", Q3 Sales=" + q3Sales + "}";
    }
}
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class ProductManager {
    private List<Product> products;

    public ProductManager() {
        this.products = new ArrayList<>();
    }

    public List<Product> getProducts() {
        return products;
    }

    public void addProduct(Scanner scanner) {
        System.out.print("请输入商品编号：");
        String id = scanner.next();
        System.out.print("请输入商品名称：");
        String name = scanner.next();
        System.out.print("请输入Q1销量：");
        int q1Sales = scanner.nextInt();
        System.out.print("请输入Q2销量：");
        int q2Sales = scanner.nextInt();
        System.out.print("请输入Q3销量：");
        int q3Sales = scanner.nextInt();
        Product product = new Product(id, name, q1Sales, q2Sales, q3Sales);
        products.add(product);
        System.out.println("商品已添加成功！");
    }

    public void deleteProduct(Scanner scanner) {
        System.out.print("请输入要删除的商品编号：");
        String id = scanner.next();
        Product productToRemove = null;
        for (Product product : products) {
            if (product.getId().equals(id)) {
                productToRemove = product;
                break;
            }
        }
        if (productToRemove != null) {
            products.remove(productToRemove);
            System.out.println("商品已成功删除！");
        } else {
            System.out.println("找不到指定编号的商品！");
        }
    }

    public void modifyProduct(Scanner scanner) {
        System.out.print("请输入要修改的商品编号：");
        String id = scanner.next();
        for (Product product : products) {
            if (product.getId().equals(id)) {
                System.out.print("请输入新的商品名称：");
                String name = scanner.next();
                System.out.print("请输入新的Q1销量：");
                int q1Sales = scanner.nextInt();
                System.out.print("请输入新的Q2销量：");
                int q2Sales = scanner.nextInt();
                System.out.print("请输入新的Q3销量：");
                int q3Sales = scanner.nextInt();
                product.setName(name);
                product.setQ1Sales(q1Sales);
                product.setQ2Sales(q2Sales);
                product.setQ3Sales(q3Sales);
                System.out.println("商品信息已成功修改！");
                return;
            }
        }
        System.out.println("找不到指定编号的商品！");
    }

    public void displayProducts() {
        if (products.isEmpty()) {
            System.out.println("暂无商品信息！");
            return;
        }

        System.out.println("编号\t\t名称\t\t第一季度销量\t第二季度销量\t第三季度销量");
        for (Product product : products) {
            System.out.printf("%s\t\t%s\t\t%d\t\t\t%d\t\t\t%d%n",
                    product.getId(),
                    product.getName(),
                    product.getQ1Sales(),
                    product.getQ2Sales(),
                    product.getQ3Sales()
            );
        }
    }

//    public void importProducts(Scanner scanner) {
//        System.out.print("请输入要导入的文件路径：");
//        String filePath = scanner.next();
//        try {
//            Scanner fileScanner = new Scanner(new File(filePath),"GBK");
//            while (fileScanner.hasNextLine()) {
//                String line = fileScanner.nextLine();
//                String[] parts = line.split(",");
//                String id = parts[0];
//                String name = parts[1];
//                int q1Sales = Integer.parseInt(parts[2]);
//                int q2Sales = Integer.parseInt(parts[3]);
//                int q3Sales = Integer.parseInt(parts[4]);
//                Product product = new Product(id, name, q1Sales, q2Sales, q3Sales);
//                products.add(product);
//            }
//            fileScanner.close();
//            System.out.println("商品信息已成功导入！");
//        } catch (IOException e) {
//            System.out.println("导入失败：" + e.getMessage());
//        }
//    }

    public void importProducts(Scanner scanner) {
        System.out.print("请输入要导入的文件路径：");
        String filePath = scanner.next();
        try {
            Scanner fileScanner = new Scanner(new File(filePath), "GBK");
//            Scanner fileScanner = new Scanner(new File(filePath),"GBK");
            while (fileScanner.hasNextLine()) {
                String line = fileScanner.nextLine();
                // 假设 txt 文件中字段之间使用空格分隔
                String[] parts = line.split("\\s+");
                String id = parts[0];
                String name = parts[1];
                int q1Sales = Integer.parseInt(parts[2]);
                int q2Sales = Integer.parseInt(parts[3]);
                int q3Sales = Integer.parseInt(parts[4]);
                Product product = new Product(id, name, q1Sales, q2Sales, q3Sales);
                products.add(product);
            }
            fileScanner.close();
            System.out.println("商品信息已成功导入！");
        } catch (IOException e) {
            System.out.println("导入失败：" + e.getMessage());
        }
    }

//    public void exportProducts() {
//        if (products.isEmpty()) {
//            System.out.println("没有商品信息可导出！");
//            return;
//        }
//        System.out.print("请输入要导出的文件路径：");
//        Scanner scanner = new Scanner(System.in);
//        String filePath = scanner.next();
//        try {
//            FileWriter writer = new FileWriter(filePath);
//            for (Product product : products) {
//                writer.write(product.getId() + "," + product.getName() + "," + product.getQ1Sales() + ","
//                        + product.getQ2Sales() + "," + product.getQ3Sales() + "\n");
//            }
//            writer.close();
//            System.out.println("商品信息已成功导出！");
//        } catch (IOException e) {
//            System.out.println("导出失败：" + e.getMessage());
//        }
//    }

//    public void exportProducts() {
//        if (products.isEmpty()) {
//            System.out.println("没有商品信息可导出！");
//            return;
//        }
//        String fileName = "products.csv"; // 导出文件名
//        try {
//            FileWriter writer = new FileWriter(fileName);
//            for (Product product : products) {
//                writer.write(product.getId() + "," + product.getName() + "," + product.getQ1Sales() + ","
//                        + product.getQ2Sales() + "," + product.getQ3Sales() + "\n");
//            }
//            writer.close();
//            System.out.println("商品信息已成功导出到 " + fileName + "！");
//        } catch (IOException e) {
//            System.out.println("导出失败：" + e.getMessage());
//        }
//    }

    public void exportProducts() {
        if (products.isEmpty()) {
            System.out.println("没有商品信息可导出！");
            return;
        }
        String fileName = "products.csv"; // 导出文件名
        try {
            FileWriter writer = new FileWriter(fileName);
            // 写入标题行
            writer.write("------------------商品信息----------------\n");
            writer.write("编号\t\t名称\t\t第一季度销量\t第二季度销量\t第三季度销量\n");
            // 写入商品信息
            for (Product product : products) {
                writer.write(product.getId() + "\t\t" + product.getName() + "\t\t" + product.getQ1Sales() + "\t\t"
                        + product.getQ2Sales() + "\t\t" + product.getQ3Sales() + "\n");
            }
            writer.write("--------------------------------------------\n");
            writer.close();
            System.out.println("商品信息已成功导出到 " + fileName + "！");
        } catch (IOException e) {
            System.out.println("导出失败：" + e.getMessage());
        }
    }


}
//
//import java.util.List;
//
//public class SalesStatistics {
//    private ProductManager productManager;
//
//    public SalesStatistics(ProductManager productManager) {
//        this.productManager = productManager;
//    }
//
//    public void highestSalesByQuarter() {
//        List<Product> productList = productManager.getProducts();
//        if (productList.isEmpty()) {
//            System.out.println("暂无商品信息！");
//            return;
//        }
//
//        for (int quarter = 1; quarter <= 3; quarter++) {
//            int maxSales = Integer.MIN_VALUE;
//            Product maxProduct = null;
//
//            for (Product product : productList) {
//                int sales = 0;
//                switch (quarter) {
//                    case 1:
//                        sales = product.getQ1Sales();
//                        break;
//                    case 2:
//                        sales = product.getQ2Sales();
//                        break;
//                    case 3:
//                        sales = product.getQ3Sales();
//                        break;
//                }
//                if (sales > maxSales) {
//                    maxSales = sales;
//                    maxProduct = product;
//                }
//            }
//
//            System.out.println("季度 " + quarter + " 销量最高的商品信息：");
//            if (maxProduct != null) {
//                System.out.println(maxProduct);
//                System.out.println("销量：" + maxSales);
//            } else {
//                System.out.println("暂无数据！");
//            }
//            System.out.println("--------------------");
//        }
//    }
//
//    public void lowestSalesByQuarter() {
//        List<Product> productList = productManager.getProducts();
//        if (productList.isEmpty()) {
//            System.out.println("暂无商品信息！");
//            return;
//        }
//
//        for (int quarter = 1; quarter <= 3; quarter++) {
//            int minSales = Integer.MAX_VALUE;
//            Product minProduct = null;
//
//            for (Product product : productList) {
//                int sales = 0;
//                switch (quarter) {
//                    case 1:
//                        sales = product.getQ1Sales();
//                        break;
//                    case 2:
//                        sales = product.getQ2Sales();
//                        break;
//                    case 3:
//                        sales = product.getQ3Sales();
//                        break;
//                }
//                if (sales < minSales) {
//                    minSales = sales;
//                    minProduct = product;
//                }
//            }
//
//            System.out.println("季度 " + quarter + " 销量最低的商品信息：");
//            if (minProduct != null) {
//                System.out.println(minProduct);
//                System.out.println("销量：" + minSales);
//            } else {
//                System.out.println("暂无数据！");
//            }
//            System.out.println("--------------------");
//        }
//    }
//
//    public void averageSalesByQuarter() {
//        List<Product> productList = productManager.getProducts();
//        if (productList.isEmpty()) {
//            System.out.println("暂无商品信息！");
//            return;
//        }
//
//        for (int quarter = 1; quarter <= 3; quarter++) {
//            int totalSales = 0;
//
//        }
//    }
//}


import java.util.List;

public class SalesStatistics {
    private ProductManager productManager;

    public SalesStatistics(ProductManager productManager) {
        this.productManager = productManager;
    }

    public void highestSalesByQuarter() {
        List<Product> productList = productManager.getProducts();
        if (productList.isEmpty()) {
            System.out.println("暂无商品信息！");
            return;
        }

        for (int quarter = 1; quarter <= 3; quarter++) {
            int maxSales = Integer.MIN_VALUE;
            Product maxProduct = null;

            for (Product product : productList) {
                int sales = 0;
                switch (quarter) {
                    case 1:
                        sales = product.getQ1Sales();
                        break;
                    case 2:
                        sales = product.getQ2Sales();
                        break;
                    case 3:
                        sales = product.getQ3Sales();
                        break;
                }
                if (sales > maxSales) {
                    maxSales = sales;
                    maxProduct = product;
                }
            }

            System.out.println("季度 " + quarter + " 销量最高的商品信息：");
            if (maxProduct != null) {
                displayProduct(maxProduct);
                System.out.println("销量：" + maxSales);
            } else {
                System.out.println("暂无数据！");
            }
            System.out.println("--------------------");
        }
    }

    public void lowestSalesByQuarter() {
        List<Product> productList = productManager.getProducts();
        if (productList.isEmpty()) {
            System.out.println("暂无商品信息！");
            return;
        }

        for (int quarter = 1; quarter <= 3; quarter++) {
            int minSales = Integer.MAX_VALUE;
            Product minProduct = null;

            for (Product product : productList) {
                int sales = 0;
                switch (quarter) {
                    case 1:
                        sales = product.getQ1Sales();
                        break;
                    case 2:
                        sales = product.getQ2Sales();
                        break;
                    case 3:
                        sales = product.getQ3Sales();
                        break;
                }
                if (sales < minSales) {
                    minSales = sales;
                    minProduct = product;
                }
            }

            System.out.println("季度 " + quarter + " 销量最低的商品信息：");
            if (minProduct != null) {
                displayProduct(minProduct);
                System.out.println("销量：" + minSales);
            } else {
                System.out.println("暂无数据！");
            }
            System.out.println("--------------------");
        }
    }

    public void averageSalesByQuarter() {
        List<Product> productList = productManager.getProducts();
        if (productList.isEmpty()) {
            System.out.println("暂无商品信息！");
            return;
        }

        for (int quarter = 1; quarter <= 3; quarter++) {
            int totalSales = 0;
            int count = 0;

            for (Product product : productList) {
                int sales = 0;
                switch (quarter) {
                    case 1:
                        sales = product.getQ1Sales();
                        break;
                    case 2:
                        sales = product.getQ2Sales();
                        break;
                    case 3:
                        sales = product.getQ3Sales();
                        break;
                }
                totalSales += sales;
                count++;
            }

            double averageSales = count > 0 ? (double) totalSales / count : 0;
            System.out.println("季度 " + quarter + " 销量的平均值为：" + averageSales);
            System.out.println("--------------------");
        }
    }

    private void displayProduct(Product product) {
        System.out.println("编号\t\t名称\t\t第一季度销量\t第二季度销量\t第三季度销量");
        System.out.printf("%s\t\t%s\t\t%d\t\t\t%d\t\t\t%d%n",
                product.getId(),
                product.getName(),
                product.getQ1Sales(),
                product.getQ2Sales(),
                product.getQ3Sales()
        );
    }
}
