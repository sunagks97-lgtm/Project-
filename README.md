#include <stdio.h>

#define MENU_SIZE 5

struct Food
{
    int id;
    char name[30];
    float price;
};

struct Food menu[MENU_SIZE] =
{
    {1, "Idli", 40.00},
    {2, "Dosa", 60.00},
    {3, "Fried Rice", 100.00},
    {4, "Biriyani", 150.00},
    {5, "Coffee", 30.00}
};

char credentials[2][20] = {"admin", "1234"};
char input[20];

int  isSpace(int c);
void readWord();
int  readInt();
int  login();
void displayMenu();
void generateBill(struct Food item, int quantity);

int main()
{
    int choice, quantity;

    printf("=========================================\n");
    printf("        ARAMANE DONNE BIRYANI            \n");
    printf("=========================================\n");

    if (login())
    {
        printf("\nLogin Successful!\n");

        displayMenu();

        printf("\nEnter your choice (1-5): ");
        choice = readInt();

        if (!(choice >= 1 && choice <= MENU_SIZE))
        {
            printf("Invalid choice!\n");
            return 0;
        }

        printf("Enter quantity: ");
        quantity = readInt();

        if (quantity <= 0)
        {
            printf("Invalid quantity!\n");
            return 0;
        }

        generateBill(menu[choice - 1], quantity);
    }
    else
    {
        printf("\nWrong Username or Password!\n");
        printf("Login Failed!\n");
    }

    return 0;
}

int isSpace(int c)
{
    return c == ' ' || c == '\n' || c == '\t' || c == '\r';
}

void readWord()
{
    int c, i = 0;

    do
    {
        c = getchar();
    } while (isSpace(c));

    while (c != EOF && !isSpace(c))
    {
        if (i < 19)
        {
            input[i] = (char)c;
            i++;
        }
        c = getchar();
    }

    input[i] = '\0';
}

int readInt()
{
    int c, value = 0, digits = 0;

    do
    {
        c = getchar();
    } while (isSpace(c));

    while (c >= '0' && c <= '9')
    {
        if (value < 100000)
        {
            value = value * 10 + (c - '0');
        }
        digits++;
        c = getchar();
    }

    while (c != '\n' && c != EOF)
    {
        c = getchar();
    }

    if (digits == 0)
    {
        return -1;
    }

    return value;
}

int login()
{
    int step, i, same, allCorrect = 1;

    printf("\n------------ LOGIN ------------\n");

    for (step = 0; step < 2; step++)
    {
        if (step == 0)
        {
            printf("Enter Username: ");
        }
        else
        {
            printf("Enter Password: ");
        }

        readWord();

        same = 1;
        i = 0;

        while (input[i] != '\0' || credentials[step][i] != '\0')
        {
            if (input[i] != credentials[step][i])
            {
                same = 0;
                break;
            }
            i++;
        }

        if (!same)
        {
            allCorrect = 0;
        }
    }

    return allCorrect;
}

void displayMenu()
{
    int i;

    printf("\n============== FOOD MENU ==============\n");

    for (i = 0; i < MENU_SIZE; i++)
    {
        printf("%d. %-12s - Rs. %.2f\n",
               menu[i].id,
               menu[i].name,
               menu[i].price);
    }

    printf("=======================================\n");
}

void generateBill(struct Food item, int quantity)
{
    float total, gst, grandTotal;

    total = item.price * quantity;
    gst = total * 0.05;
    grandTotal = total + gst;

    printf("\n=========================================\n");
    printf("        ARAMANE DONNE BIRYANI            \n");
    printf("            OFFICIAL RECEIPT             \n");
    printf("=========================================\n");

    printf("Item Ordered  : %s\n", item.name);
    printf("Unit Price    : Rs. %.2f\n", item.price);
    printf("Quantity      : %d\n", quantity);

    printf("-----------------------------------------\n");
    printf("Subtotal      : Rs. %.2f\n", total);
    printf("GST (5%%)      : Rs. %.2f\n", gst);

    printf("-----------------------------------------\n");
    printf("GRAND TOTAL   : Rs. %.2f\n", grandTotal);

    printf("=========================================\n");
    printf("       THANK YOU! PLEASE VISIT AGAIN     \n");
    printf("=========================================\n");
}
