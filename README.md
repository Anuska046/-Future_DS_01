"Future_DS_01"



#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_LINE 1024
#define MAX_PRODUCTS 100
#define MAX_CATEGORIES 50

typedef struct {
    char name[50];
    int quantity;
    float revenue;
} Product;

typedef struct {
    char name[50];
    float total_revenue;
} Category;

Product products[MAX_PRODUCTS];
Category categories[MAX_CATEGORIES];
int product_count = 0;
int category_count = 0;

// Helper to find or add product
int find_or_add_product(const char* name) {
    for (int i = 0; i < product_count; i++) {
        if (strcmp(products[i].name, name) == 0)
            return i;
    }
    strcpy(products[product_count].name, name);
    products[product_count].quantity = 0;
    products[product_count].revenue = 0;
    return product_count++;
}

// Helper to find or add category
int find_or_add_category(const char* name) {
    for (int i = 0; i < category_count; i++) {
        if (strcmp(categories[i].name, name) == 0)
            return i;
    }
    strcpy(categories[category_count].name, name);
    categories[category_count].total_revenue = 0;
    return category_count++;
}

int main() {
    FILE* file = fopen("sales.csv", "r");
    if (!file) {
        printf("Could not open sales.csv\n");
        return 1;
    }

    char line[MAX_LINE];
    fgets(line, MAX_LINE, file);  // Skip header

    while (fgets(line, MAX_LINE, file)) {
        char* token;
        char product[50], category[50];
        int quantity;
        float revenue;

        // Parse product
        token = strtok(line, ",");
        strcpy(product, token);

        // Parse category
        token = strtok(NULL, ",");
        strcpy(category, token);

        // Parse quantity
        token = strtok(NULL, ",");
        quantity = atoi(token);

        // Parse revenue
        token = strtok(NULL, ",");
        revenue = atof(token);

        // Update product
        int prod_index = find_or_add_product(product);
        products[prod_index].quantity += quantity;
        products[prod_index].revenue += revenue;

        // Update category
        int cat_index = find_or_add_category(category);
        categories[cat_index].total_revenue += revenue;
    }

    fclose(file);

    // Find best-selling and top-revenue product
    int best_index = 0, top_index = 0;
    for (int i = 1; i < product_count; i++) {
        if (products[i].quantity > products[best_index].quantity)
            best_index = i;
        if (products[i].revenue > products[top_index].revenue)
            top_index = i;
    }

    // Find top revenue category
    int top_cat_index = 0;
    for (int i = 1; i < category_count; i++) {
        if (categories[i].total_revenue > categories[top_cat_index].total_revenue)
            top_cat_index = i;
    }

    // Output results
    printf("\nBest-selling Product: %s (Sold: %d units)\n", products[best_index].name, products[best_index].quantity);
    printf("Top Revenue Product: %s (Revenue: $%.2f)\n", products[top_index].name, products[top_index].revenue);
    printf("Top Revenue Category: %s (Revenue: $%.2f)\n", categories[top_cat_index].name, categories[top_cat_index].total_revenue);

    return 0;
}