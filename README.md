#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define MAX_SIZE 10
#define BORDER_WIDTH 60

// Stack structure using a fixed-size array
typedef struct {
    int items[MAX_SIZE];
    int top;
} Stack;

// Core stack operations
void initStack(Stack* s);
bool isEmpty(Stack* s);
bool isFull(Stack* s);
void push(Stack* s, int value);
int pop(Stack* s);
int peek(Stack* s);

// Display / visualization helpers
void printBorder();
void printSectionHeader(const char* title);
void printStackVisualization(Stack* s, const char* operation);
void showStackState(Stack* s);
void pauseScreen();
void displayMenu();

// Initialize stack (top = -1 means empty)
void initStack(Stack* s) {
    s->top = -1;
    printf("\n");
    printSectionHeader("STACK INITIALIZATION");
    printf("  → Setting top = -1 (indicates empty stack)\n");
    printf("  → Stack is now ready to accept elements\n");
    printf("  → Maximum capacity: %d elements\n", MAX_SIZE);
}

// Return true if stack has no elements
bool isEmpty(Stack* s) {
    bool result = (s->top == -1);
    
    printf("\n");
    printSectionHeader("CHECKING IF STACK IS EMPTY");
    printf("  LOGIC: if (top == -1) then stack is EMPTY\n\n");
    printf("  → Current top value: %d\n", s->top);
    printf("  → Comparing: top == -1 ? %s\n", result ? "TRUE" : "FALSE");
    printf("  → Result: Stack is %s\n", result ? "EMPTY ✓" : "NOT EMPTY ✗");
    
    if (result) {
        printf("\n  EXPLANATION: top = -1 means no elements in stack\n");
    } else {
        printf("\n  EXPLANATION: top = %d means %d element(s) in stack\n", 
               s->top, s->top + 1);
    }
    
    return result;
}

// Return true if stack is at maximum capacity
bool isFull(Stack* s) {
    bool result = (s->top == MAX_SIZE - 1);
    
    printf("\n");
    printSectionHeader("CHECKING IF STACK IS FULL");
    printf("  LOGIC: if (top == MAX_SIZE-1) then stack is FULL\n\n");
    printf("  → Maximum size: %d\n", MAX_SIZE);
    printf("  → Maximum top index: %d (MAX_SIZE - 1)\n", MAX_SIZE - 1);
    printf("  → Current top value: %d\n", s->top);
    printf("  → Comparing: top == %d ? %s\n", MAX_SIZE - 1, result ? "TRUE" : "FALSE");
    printf("  → Result: Stack is %s\n", result ? "FULL ✓" : "NOT FULL ✗");
    
    if (result) {
        printf("\n  EXPLANATION: All %d positions are occupied\n", MAX_SIZE);
    } else {
        printf("\n  EXPLANATION: %d positions available (%d/%d used)\n", 
               MAX_SIZE - (s->top + 1), s->top + 1, MAX_SIZE);
    }
    
    return result;
}

// Push a new value on top of the stack
void push(Stack* s, int value) {
    printf("\n");
    printSectionHeader("PUSH OPERATION");
    printf("  Attempting to PUSH value: %d\n", value);
    
    printf("\n  BEFORE PUSH:\n");
    showStackState(s);
    
    printf("\n  STEP 1: Check if stack is FULL\n");
    if (isFull(s)) {
        printf("\n  ❌ STACK OVERFLOW DETECTED!\n");
        printf("     Cannot push %d - stack is at maximum capacity\n", value);
        printStackVisualization(s, "PUSH FAILED - OVERFLOW");
        return;
    }
    
    printf("     ✓ Stack is not full, proceed with push\n");
    
    printf("\n  STEP 2: Increment top pointer\n");
    printf("     Old top: %d\n", s->top);
    s->top++;
    printf("     New top: %d (top++)\n", s->top);
    
    printf("\n  STEP 3: Insert value at position top\n");
    printf("     items[%d] = %d\n", s->top, value);
    s->items[s->top] = value;
    
    printf("\n  ✓ PUSH SUCCESSFUL!\n");
    printf("    Value %d added at index %d\n", value, s->top);
    
    printf("\n  AFTER PUSH:\n");
    showStackState(s);
    
    printStackVisualization(s, "AFTER PUSH");
}

// Pop and return the top value (if any)
int pop(Stack* s) {
    printf("\n");
    printSectionHeader("POP OPERATION");
    printf("  Attempting to POP top element\n");
    
    printf("\n  BEFORE POP:\n");
    showStackState(s);
    
    printf("\n  STEP 1: Check if stack is EMPTY\n");
    if (isEmpty(s)) {
        printf("\n  ❌ STACK UNDERFLOW DETECTED!\n");
        printf("     Cannot pop - stack is empty\n");
        printStackVisualization(s, "POP FAILED - UNDERFLOW");
        return -1;
    }
    
    printf("     ✓ Stack is not empty, proceed with pop\n");
    
    printf("\n  STEP 2: Retrieve value at top\n");
    int poppedValue = s->items[s->top];
    printf("     poppedValue = items[%d] = %d\n", s->top, poppedValue);
    
    printf("\n  STEP 3: Decrement top pointer\n");
    printf("     Old top: %d\n", s->top);
    s->top--;
    printf("     New top: %d (top--)\n", s->top);
    
    printf("\n  ✓ POP SUCCESSFUL!\n");
    printf("    Removed value: %d\n", poppedValue);
    printf("    This value is now inaccessible (still in array but top moved)\n");
    
    printf("\n  AFTER POP:\n");
    showStackState(s);
    
    printStackVisualization(s, "AFTER POP");
    
    return poppedValue;
}

// Return the top value without removing it
int peek(Stack* s) {
    printf("\n");
    printSectionHeader("PEEK OPERATION");
    printf("  Attempting to PEEK at top element (without removing)\n");
    
    showStackState(s);
    
    printf("\n  STEP 1: Check if stack is EMPTY\n");
    if (isEmpty(s)) {
        printf("\n  ❌ PEEK FAILED!\n");
        printf("     Cannot peek - stack is empty\n");
        return -1;
    }
    
    printf("     ✓ Stack is not empty, proceed with peek\n");
    
    printf("\n  STEP 2: Read value at top (WITHOUT removing)\n");
    printf("     topValue = items[%d] = %d\n", s->top, s->items[s->top]);
    
    printf("\n  ✓ PEEK SUCCESSFUL!\n");
    printf("    Top element: %d\n", s->items[s->top]);
    printf("    Note: Element remains in stack (only viewed, not removed)\n");
    
    printStackVisualization(s, "PEEK - VIEW ONLY");
    
    return s->items[s->top];
}

// Simple border for visual separation
void printBorder() {
    for (int i = 0; i < BORDER_WIDTH; i++) {
        printf("=");
    }
    printf("\n");
}

// Print a formatted section title
void printSectionHeader(const char* title) {
    printBorder();
    printf("  %s\n", title);
    printBorder();
}

// Show basic status of the stack (top, size, empty/full)
void showStackState(Stack* s) {
    printf("     Current top: %d\n", s->top);
    printf("     Stack size: %d/%d\n", s->top + 1, MAX_SIZE);
    printf("     Status: %s | %s\n", 
           isEmpty(s) ? "EMPTY" : "HAS ELEMENTS",
           isFull(s) ? "FULL" : "SPACE AVAILABLE");
}

// Visual representation of stack contents and indices
void printStackVisualization(Stack* s, const char* operation) {
    printf("\n");
    printSectionHeader(operation);
    
    printf("\n  MEMORY LAYOUT (Array indices 0 to %d):\n\n", MAX_SIZE - 1);
    
    printf("     Index: ");
    for (int i = 0; i < MAX_SIZE; i++) {
        printf("[%2d]", i);
    }
    printf("\n");
    
    printf("     Value: ");
    for (int i = 0; i < MAX_SIZE; i++) {
        if (i <= s->top) {
            printf("[%2d]", s->items[i]);
        } else {
            printf("[  ]");
        }
    }
    printf("\n");
    
    printf("            ");
    for (int i = 0; i < s->top; i++) {
        printf("    ");
    }
    if (s->top >= 0) {
        printf(" ↑\n");
        printf("            ");
        for (int i = 0; i < s->top; i++) {
            printf("    ");
        }
        printf("TOP\n");
    } else {
        printf("(empty)\n");
    }
    
    printf("\n  VISUAL STACK (LIFO - Last In, First Out):\n\n");
    
    if (isEmpty(s)) {
        printf("     ┌─────────────┐\n");
        printf("     │   EMPTY     │\n");
        printf("     └─────────────┘\n");
    } else {
        printf("        TOP ↓\n");
        
        for (int i = s->top; i >= 0; i--) {
            printf("     ┌─────────────┐\n");
            printf("     │  %5d      │", s->items[i]);
            
            if (i == s->top) {
                printf(" ← TOP (index %d) - LIFO: Last In, First Out", i);
            } else if (i == 0) {
                printf(" ← BOTTOM (index %d)", i);
            }
            printf("\n");
        }
        printf("     └─────────────┘\n");
    }
    
    printf("\n  STACK INFORMATION:\n");
    printf("     • Elements in stack: %d\n", s->top + 1);
    printf("     • Top index: %d\n", s->top);
    printf("     • Capacity: %d\n", MAX_SIZE);
    printf("     • Space remaining: %d\n", MAX_SIZE - (s->top + 1));
    printf("     • Is Empty: %s\n", isEmpty(s) ? "Yes" : "No");
    printf("     • Is Full: %s\n", isFull(s) ? "Yes" : "No");
    
    printBorder();
}

// Simple "press Enter to continue" pause
void pauseScreen() {
    printf("\n>>> Press Enter to continue...");
    while (getchar() != '\n');
}

// Menu shown to the user for operations
void displayMenu() {
    printf("\n\n");
    printBorder();
    printf("  STACK OPERATIONS MENU\n");
    printBorder();
    printf("\n");
    printf("  1. PUSH    - Add element to top of stack\n");
    printf("  2. POP     - Remove element from top of stack\n");
    printf("  3. PEEK    - View top element (without removing)\n");
    printf("  4. isEmpty - Check if stack is empty\n");
    printf("  5. isFull  - Check if stack is full\n");
    printf("  6. DISPLAY - Show current stack state\n");
    printf("  7. EXIT    - Exit program\n");
    printf("\n");
    printBorder();
    printf("  Enter your choice (1-7): ");
}

int main() {
    Stack stack;
    int choice, value;
    
    // Intro section for the simulator
    printf("\n");
    printBorder();
    printf("  WELCOME TO STACK SIMULATOR - EDUCATIONAL VERSION\n");
    printBorder();
    printf("\n  This program demonstrates how a STACK works internally.\n");
    printf("  Each operation shows step-by-step execution.\n");
    printf("\n  Key Concept: STACK follows LIFO (Last In, First Out)\n");
    printf("  - Last element pushed is the first to be popped\n");
    printf("  - Only the TOP element can be accessed/removed\n");
    printf("\n  Maximum Capacity: %d elements\n", MAX_SIZE);
    printBorder();
    
    initStack(&stack);
    pauseScreen();
    
    // Main menu loop
    while (true) {
        displayMenu();
        
        if (scanf("%d", &choice) != 1) {
            while (getchar() != '\n');
            printf("\n  ❌ Invalid input! Please enter a number (1-7).\n");
            pauseScreen();
            continue;
        }
        while (getchar() != '\n');
        
        switch (choice) {
            case 1:
                printf("\n  Enter integer value to push: ");
                if (scanf("%d", &value) != 1) {
                    while (getchar() != '\n');
                    printf("\n  ❌ Invalid input! Please enter an integer.\n");
                    pauseScreen();
                    break;
                }
                while (getchar() != '\n');
                push(&stack, value);
                pauseScreen();
                break;
                
            case 2:
                pop(&stack);
                pauseScreen();
                break;
                
            case 3:
                peek(&stack);
                pauseScreen();
                break;
                
            case 4:
                isEmpty(&stack);
                printStackVisualization(&stack, "CURRENT STACK STATE");
                pauseScreen();
                break;
                
            case 5:
                isFull(&stack);
                printStackVisualization(&stack, "CURRENT STACK STATE");
                pauseScreen();
                break;
                
            case 6:
                printf("\n");
                printSectionHeader("DISPLAY STACK");
                printf("\n  Showing complete stack state...\n");
                showStackState(&stack);
                printStackVisualization(&stack, "CURRENT STACK STATE");
                pauseScreen();
                break;
                
            case 7:
                printf("\n");
                printBorder();
                printf("  Thank you for using Stack Simulator!\n");
                printf("  Hope you understood how STACK works internally.\n");
                printBorder();
                printf("\n");
                exit(0);
                
            default:
                printf("\n  ❌ Invalid choice! Please select 1-7.\n");
                pauseScreen();
        }
    }
    
    return 0;
}

