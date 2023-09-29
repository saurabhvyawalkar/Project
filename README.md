# Project
Measure 0 to 100Vdc using micro controller 
#include <xc.h>
#include <stdint.h>
// Configuration settings (adjust according to your setup)
#pragma config FOSC = HS     // High-speed crystal oscillator
#pragma config WDTE = OFF    // Watchdog timer disabled
#pragma config PWRTE = OFF   // Power-up timer disabled
#pragma config BOREN = ON    // Brown-out reset enabled
#pragma config LVP = OFF     // Low-voltage programming disabled

// Function to initialize ADC
void ADC_Init() {
    ADCON0 = 0b00000001; // Select channel AN0 and enable ADC
    ADCON1 = 0b10000000; // Use VDD as the reference voltage
}

// Function to read ADC value
uint16_t ADC_Read() {
    ADCON0bits.GO = 1;   // Start ADC conversion
    
    while (ADCON0bits.GO); // Wait for conversion to complete
    
    return ((uint16_t)ADRESH << 8) | ADRESL; // Combine high and low bytes
}

void main() {
    TRISA = 0xFF;   // Set PORTA as input (assuming AN0 is connected to PORTA)
    
    // Initialize ADC
    ADC_Init();
    
 // Variables
    uint16_t adcValue;
    float voltage;

    while (1) {
        // Read ADC value
        adcValue = ADC_Read();
        // Convert ADC value to voltage (0-100V range)
        voltage = (adcValue * 5.0) / 1024.0; // Assuming VDD is 5V
         
         // Add a delay before taking the next reading (optional)
        __delay_ms(1000); // Delay for 1 second
    }
}

