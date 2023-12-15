//VANSH DAVE
//C PROJECT (PRG255)
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_ITEMS 12
#define MAX_NAME_LENGTH 50
#define FILENAME "inventory_data.txt"

struct Item {       // Struct to store information about an item
    char name[MAX_NAME_LENGTH];
    int idNumber;
    double cost;
    int quantityInStock;
    int quantitySold;
    double totalProfit;
};

struct Inventory {
    struct Item items[MAX_ITEMS];
    double totalStoreProfit;
};

// Function prototypes
void displayMenu();
void showInventory(struct Inventory *inventory);
void sellItem(struct Inventory *inventory);
void showProfit(struct Inventory *inventory);
void orderItem(struct Inventory *inventory);
void orderNewItem(struct Inventory *inventory);
void saveToFile(struct Inventory *inventory);
void loadFromFile(struct Inventory *inventory);

int main() {
    struct Inventory inventory;
    loadFromFile(&inventory);

    char choice;
    do {
        displayMenu();
        printf("Enter your choice: ");
        scanf(" %c", &choice);

        switch (choice) {
            case 'a':
                showInventory(&inventory);
                break;
            case 'b':
                sellItem(&inventory);
                break;
            case 'c':
                showProfit(&inventory);
                break;
            case 'd':
                orderItem(&inventory);
                break;
            case 'e':
                orderNewItem(&inventory);
                break;
            case 'f':
                saveToFile(&inventory);
                break;
            default:
                printf("Invalid choice. Please enter a valid option.\n");
        }

    } while (choice != 'f');

    return 0;
}

void displayMenu() {
    printf("\n \t \tInventory Control System\n \n\n");
    printf(" \t To choose a function, enter its letter label:\n");
    printf(" \t a) Show items in inventory\n");
    printf(" \t b) Sell an item\n");
    printf(" \t c) Show profit\n");
    printf(" \t d) Order more of an existing item\n");
    printf(" \t e) Order new items\n");
    printf(" \t f) Quit\n");
}

void showInventory(struct Inventory *inventory) {  
    printf("\n \t \t Inventory Control System\n");
    printf("Item Name   Id Number   Quantity   Unit Cost   Value\n");

    for (int i = 0; i < MAX_ITEMS; i++) {
        if (strlen(inventory->items[i].name) > 0) {
            double value = inventory->items[i].cost * inventory->items[i].quantityInStock;
            printf("%-12s%-12d%-12d$%-11.2lf$%.2lf\n",
                   inventory->items[i].name, inventory->items[i].idNumber,
                   inventory->items[i].quantityInStock, inventory->items[i].cost, value);
        }
    }
}

void sellItem(struct Inventory *inventory) {
    int itemId, quantitySold;
    printf("Enter the item ID to sell: ");
    scanf("%d", &itemId);

    for (int i = 0; i < MAX_ITEMS; i++) {
        if (inventory->items[i].idNumber == itemId) {
            printf("Enter the quantity sold: ");
            scanf("%d", &quantitySold);

            if (quantitySold > 0 && quantitySold <= inventory->items[i].quantityInStock) {
                inventory->items[i].quantityInStock -= quantitySold;
                inventory->items[i].quantitySold += quantitySold;
                double profit = (inventory->items[i].cost * 1.5) * quantitySold;
                inventory->items[i].totalProfit += profit;
                // inventory->totalStoreProfit += profit;
                printf("Sale completed successfully.\n");
            } else {
                printf("Invalid quantity sold. Sale canceled.\n");
            }
            return;
        }
    }

    printf("Item not found.\n");
}

void showProfit(struct Inventory *inventory) {
    printf("\n \t \t Inventory Control System\n");
    printf("Item Name   Id Number   Quantity Sold   Item Profit\n");

    double totalProfit = 0.0;

    for (int i = 0; i < MAX_ITEMS; i++) {
        if (strlen(inventory->items[i].name) > 0) {
            double itemProfit = inventory->items[i].totalProfit;
            totalProfit += itemProfit;
            printf("%-12s%-12d%-16d$%.2lf\n",
                   inventory->items[i].name, inventory->items[i].idNumber,
                   inventory->items[i].quantitySold, itemProfit);
        }
    }

    printf("TotalProfit: $%.2lf\n", totalProfit);
}

void orderItem(struct Inventory *inventory) {
    int itemId, quantityOrdered;
    printf("Enter the item ID to order: ");
    scanf("%d", &itemId);

    for (int i = 0; i < MAX_ITEMS; i++) {
        if (inventory->items[i].idNumber == itemId) {
            printf("Enter the quantity to order: ");
            scanf("%d", &quantityOrdered);

            if (quantityOrdered > 0) {
                inventory->items[i].quantityInStock += quantityOrdered;
                printf("Order placed successfully.\n");
            } else {
                printf("Invalid quantity ordered. Order canceled.\n");
            }
            return;
        }
    }

    printf("Item not found.\n");
}

void orderNewItem(struct Inventory *inventory) {
    for (int i = 0; i < MAX_ITEMS; i++) {
        if (strlen(inventory->items[i].name) == 0) {
            printf("Enter the name of the new item: ");
            scanf("%s", inventory->items[i].name);
            printf("Enter the item ID: ");
            scanf("%d", &inventory->items[i].idNumber);
            printf("Enter the cost of the item: $");
            scanf("%lf", &inventory->items[i].cost);
            printf("Enter the initial quantity in stock: ");
            scanf("%d", &inventory->items[i].quantityInStock);
            printf("New item added successfully.\n");
            return;
        }
    }

    printf("Maximum number of items reached. Cannot add a new item.\n");
}

void saveToFile(struct Inventory *inventory) {
    FILE *file = fopen(FILENAME, "w");
    if (file == NULL) {
        printf("Error opening file for writing.\n");
        return;
    }

    for (int i = 0; i < MAX_ITEMS; i++) {
        if (strlen(inventory->items[i].name) > 0) {
            fprintf(file, "%s %d %.2lf %d %d %.2lf\n",
                    inventory->items[i].name, inventory->items[i].idNumber,
                    inventory->items[i].cost, inventory->items[i].quantityInStock,
                    inventory->items[i].quantitySold, inventory->items[i].totalProfit);
        }
    }

    fclose(file);
}
void loadFromFile(struct Inventory *inventory) {
    FILE *file = fopen(FILENAME, "r");
    if (file == NULL) {
        printf("File not found. Starting with an empty inventory.\n");
        return;
    }

    for (int i = 0; i < MAX_ITEMS; i++) {
        if (fscanf(file, "%s %d %lf %d %d %lf",
                   inventory->items[i].name, &inventory->items[i].idNumber,
                   &inventory->items[i].cost, &inventory->items[i].quantityInStock,
                   &inventory->items[i].quantitySold, &inventory->items[i].totalProfit) == 6 &&
            inventory->items[i].cost >= 0) {
            // Successfully read valid data
        } else {
            // Invalid data, set default values
            strcpy(inventory->items[i].name, "");
            inventory->items[i].idNumber = 0;
            inventory->items[i].cost = 0.0;
            inventory->items[i].quantityInStock = 0;
            inventory->items[i].quantitySold = 0;
            inventory->items[i].totalProfit = 0.0;
        }
    }

    fclose(file);
}
