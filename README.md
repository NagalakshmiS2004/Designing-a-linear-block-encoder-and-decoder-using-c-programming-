# Designing-a-linear-block-encoder-and-decoder-using-c-programming-
A block code is an error-correcting code where a fixed-length block of message bits is transformed into  a longer block of codeword bits. The primary goal of this transformation is to introduce redundancy into  the original message so that errors introduced during transmission or storage can be detected and  corrected. 


CODE : #include <stdio.h>

#define K 4 

#define N 7 

#define M (N - K) 

void matrix_multiply_mod2(int message[], int matrix[][N], int result[]) {

 for (int i = 0; i < N; i++) {

 result[i] = 0; // Initialize result to 0

 for (int j = 0; j < K; j++) {

 result[i] ^= (message[j] & matrix[j][i]); // XOR operation }}}

 void calculate_syndrome(int received[], int H[][N], int syndrome[]) {

 for (int i = 0; i < M; i++) {

 syndrome[i] = 0; // Initialize syndrome to 0

 for (int j = 0; j < N; j++) {

 syndrome[i] ^= (received[j] & H[i][j]); // XOR operation}}}

int main() {

 int G[4][7] = {

 {1, 0, 0, 0, 1, 1, 1}, // Row 1

 {0, 1, 0, 0, 1, 1, 0}, // Row 2

 {0, 0, 1, 0, 1, 0, 1}, // Row 3

 {0, 0, 0, 1, 0, 1, 1} // Row 4

 };

 

 int message[K] = {1, 0, 1, 0}; 
 int codeword[N] = {0}; // To store the encoded codeword

 matrix_multiply_mod2(message, G, codeword);

 printf("Generated Codeword: ");

 for (int i = 0; i < N; i++) {

 printf("%d ", codeword[i]);

 }

 printf("\n");

 int error_vector[N] = {0, 0, 1, 0, 0, 0, 0}; // Error in the 3rd bit

 int received_codeword[N]; // Received codeword with error

 for (int i = 0; i < N; i++) {

 received_codeword[i] = (codeword[i] ^ error_vector[i]) % 2; // Add error to the codeword

 }

 printf("Received Codeword (with Error): ");

 for (int i = 0; i < N; i++) {

 printf("%d ", received_codeword[i]);

 }

 printf("\n");

 int H[3][7] = {

 {1, 1, 1, 0, 1, 0, 0}, // Row 1

 {1, 1, 0, 1, 0, 1, 0}, // Row 2

 {1, 0, 1, 1, 0, 0, 1} // Row 3

 };
 int syndrome[M]; // To store the syndrome

 calculate_syndrome(received_codeword, H, syndrome);

 printf("Syndrome: ");

 for (int i = 0; i < M; i++) {

 printf("%d ", syndrome[i]);

 }

 printf("\n");

 int error_detected = 0;

 for (int i = 0; i < M; i++) {

 if (syndrome[i] != 0) {

 error_detected = 1;

 break;

 }}

 if (error_detected) {

 printf("Error detected in the received codeword.\n");{ else {

 printf("No error detected.\n"); }

 }return 0; }
 
 
 
 SIMULATION OUTPUT:

Generated Codeword: 1 0 1 0 1 1 1

Received Codeword (with Error): 1 0 0 0 1 1 1

Syndrome: 1 0 1 

Error detected in the received codeword.
