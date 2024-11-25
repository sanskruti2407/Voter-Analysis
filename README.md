# Voter-Analysis
#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Define structure for a candidate
struct Candidate {
    long long candidate_id; // Increased size to accommodate up to 10 digits
    char name[50];
    int votes;
};

// Function to sort candidates by votes and handle ties by name
void sortCandidates(struct Candidate candidates[], int n) {
    struct Candidate temp;
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (candidates[j].votes < candidates[j + 1].votes || 
                (candidates[j].votes == candidates[j + 1].votes && 
                 strcmp(candidates[j].name, candidates[j + 1].name) > 0)) {
                temp = candidates[j];
                candidates[j] = candidates[j + 1];
                candidates[j + 1] = temp;
            }
        }
    }
}

// Function to display the sorted candidates
void displayResults(struct Candidate candidates[], int n) {
    printf("\nElection Results:\n");
    printf("-------------------------------------------------\n");
    printf("| Candidate ID | Name                | Votes   |\n");
    printf("-------------------------------------------------\n");
    for (int i = 0; i < n; i++) {
        printf("| %-13lld | %-18s | %-7d |\n", candidates[i].candidate_id, candidates[i].name, candidates[i].votes);
    }
    printf("-------------------------------------------------\n");

    // Declare the winner
    printf("\nWinner: %s with %d votes\n", candidates[0].name, candidates[0].votes);
}

// Function to validate candidate ID
int isValidCandidateID(long long candidate_id) {
    return (candidate_id > 0 && candidate_id <= 9999999999LL);
}

int main() {
    int n;

    // Input number of candidates
    do {
        printf("Enter the number of candidates (positive number): ");
        scanf("%d", &n);
        if (n <= 0) {
            printf("Error: Number of candidates must be a positive number.\n");
        }
    } while (n <= 0);

    // Array to store candidate details
    struct Candidate candidates[n];

    // Input details for each candidate
    for (int i = 0; i < n; i++) {
        printf("\nEnter details for candidate %d:\n", i + 1);

        // Input and validate Candidate ID
        do {
            printf("Candidate ID (1-10 digit numeric only): ");
            scanf("%lld", &candidates[i].candidate_id);

            if (!isValidCandidateID(candidates[i].candidate_id)) {
                printf("Error: Candidate ID must be a numeric value with up to 10 digits.\n");
            }
        } while (!isValidCandidateID(candidates[i].candidate_id));

        printf("Name: ");
        getchar(); // Clear the buffer
        fgets(candidates[i].name, sizeof(candidates[i].name), stdin);
        candidates[i].name[strcspn(candidates[i].name, "\n")] = '\0'; // Remove newline character

        // Input and validate votes
        do {
            printf("Votes (positive number): ");
            scanf("%d", &candidates[i].votes);
            if (candidates[i].votes < 0) {
                printf("Error: Votes cannot be a negative number.\n");
            }
        } while (candidates[i].votes < 0);
    }

    // Sort candidates by votes
    sortCandidates(candidates, n);

    // Display results
    displayResults(candidates, n);

    return 0;
}
