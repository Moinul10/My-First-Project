# My-First-Project#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>

#define Rock 1
#define Paper 2
#define Scissors 3
#define Lizard 4
#define Spock 5

void displayRules();
int getPlayerChoice();
int getComputerChoice();
void determineWinner(int playerChoice, int computerChoice, int *playerScore, int *computerScore);
const char* getChoiceName(int choice);

int main() {
    int playerScore = 0, computerScore = 0;
    char playerName[50];
    int rounds, currentRound = 1;
    char playAgain;
    char restartGame;

    srand(time(NULL)); // Seed the random number generator

    printf("Welcome to Rock-Paper-Scissors-Lizard-Spock!\n\n");
    printf("Enter your name: ");
    fgets(playerName, sizeof(playerName), stdin);
    playerName[strcspn(playerName, "\n")] = '\0'; // Remove newline character

    do {
        playerScore = 0;
        computerScore = 0;

        printf("\nHow many rounds would you like to play (Best of N)? :");
        scanf("%d", &rounds);
        getchar(); // Consume newline character

        while (currentRound <= rounds) {
            printf("\nRound %d/%d\n", currentRound, rounds);
            displayRules();

            int playerChoice = getPlayerChoice();

            if (playerChoice == 0) {
                printf("\nWould you like to restart the game? (y/n): ");
                scanf(" %c", &restartGame);
                getchar(); // Consume newline character
                if (restartGame == 'y' || restartGame == 'Y') {
                    currentRound = 1;
                    playerScore = playerScore;
                    computerScore = computerScore ;
                    break;
                } else {
                    continue;
                }
            }

            int computerChoice = getComputerChoice();

            printf("\n%s chose: %s\n", playerName, getChoiceName(playerChoice));
            printf("Computer chose: %s\n", getChoiceName(computerChoice));

            determineWinner(playerChoice, computerChoice, &playerScore, &computerScore);

            printf("\nCurrent Score:\n%s: %d\nComputer: %d\n", playerName, playerScore, computerScore);
            currentRound++;
        }

        printf("\nFinal Score:\n%s: %d\nComputer: %d\n", playerName, playerScore, computerScore);

        if (playerScore > computerScore) {
            printf("Congratulations, %s! You won the game!\n", playerName);
        } else if (playerScore < computerScore) {
            printf("The computer wins this time. Better luck next time, %s!\n", playerName);
        } else {
            printf("It's a tie! Great match, %s!\n", playerName);
        }

        printf("\nWould you like to play again? (y/n): ");
        scanf(" %c", &playAgain);
        getchar(); // Consume newline character
        currentRound = 1;

    } while (playAgain == 'y' || playAgain == 'Y');

    printf("\nThank you for playing, %s! Goodbye!\n", playerName);
    return 0;
}

void displayRules() {
    printf("\nGame Rules:\n");
    printf("1. Rock crushes Scissors and Lizard\n");
    printf("2. Paper covers Rock and disproves Spock\n");
    printf("3. Scissors cut Paper and decapitate Lizard\n");
    printf("4. Lizard eats Paper and poisons Spock\n");
    printf("5. Spock smashes Scissors and vaporizes Rock\n\n");
}

int getPlayerChoice() {
    int choice;
    do {
        printf("Enter your choice (1=Rock, 2=Paper, 3=Scissors, 4=Lizard, 5=Spock, 0=Quit): ");
        scanf("%d", &choice);
        getchar(); // Consume newline character

        if (choice < 0 || choice > 5) {
            printf("Invalid choice. Please choose a number between 0 and 5.\n");
        }
    } while (choice < 0 || choice > 5);

    return choice;
}

int getComputerChoice() {
    return (rand() % 5) + 1; // Random number between 1 and 5
}

void determineWinner(int playerChoice, int computerChoice, int *playerScore, int *computerScore) {
    if (playerChoice == computerChoice) {
        printf("It's a tie!\n");
    } else if ((playerChoice == Rock && (computerChoice == Scissors || computerChoice == Lizard)) ||
               (playerChoice == Paper && (computerChoice == Rock || computerChoice == Spock)) ||
               (playerChoice == Scissors && (computerChoice == Paper || computerChoice == Lizard)) ||
               (playerChoice == Lizard && (computerChoice == Paper || computerChoice == Spock)) ||
               (playerChoice == Spock && (computerChoice == Scissors || computerChoice == Rock))) {
        printf("You win this round!\n");
        (*playerScore)++;
    } else {
        printf("Computer wins this round!\n");
        (*computerScore)++;
    }
}

const char* getChoiceName(int choice) {
    switch (choice) {
        case Rock: return "Rock";
        case Paper: return "Paper";
        case Scissors: return "Scissors";
        case Lizard: return "Lizard";
        case Spock: return "Spock";
        default: return "Unknown";
    }
}
