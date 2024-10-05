% MATLAB Program for Hamming Code (7,4)

% Clear all variables and close figures
clear all;
close all;
clc;

% Input: 4-bit message
msg = input('Enter a 4-bit binary message (as a row vector): ');

% Check if the input is valid (4-bit binary vector)
if length(msg) ~= 4 || ~all(ismember(msg, [0 1]))
    error('Input must be a 4-bit binary vector.');
end

% Initialize Hamming Code (7,4) Parity positions
% Bits: [p1 p2 d1 p3 d2 d3 d4] (p = parity, d = data)
p1 = 0; p2 = 0; p3 = 0; % Initialize parity bits

% Assign data bits
d1 = msg(1);
d2 = msg(2);
d3 = msg(3);
d4 = msg(4);

% Calculate parity bits based on data bits
p1 = xor(xor(d1, d2), d4); % Parity for positions 1, 3, 5, 7
p2 = xor(xor(d1, d3), d4); % Parity for positions 2, 3, 6, 7
p3 = xor(xor(d2, d3), d4); % Parity for positions 4, 5, 6, 7

% Form the Hamming codeword
hamming_code = [p1 p2 d1 p3 d2 d3 d4];

% Display the generated Hamming code
disp('Generated Hamming Code (7,4): ');
disp(hamming_code);

% Simulate error by flipping one bit (optional)
error_sim = input('Do you want to simulate a single-bit error? (1 for yes, 0 for no): ');
if error_sim
    bit_to_flip = input('Enter the bit position (1-7) to flip: ');
    if bit_to_flip >= 1 && bit_to_flip <= 7
        hamming_code(bit_to_flip) = ~hamming_code(bit_to_flip); % Flip the bit
        disp('Hamming Code with simulated error: ');
        disp(hamming_code);
    else
        error('Invalid bit position. Enter a number between 1 and 7.');
    end
end

% Error detection and correction
% Recalculate parity bits for received code
r1 = xor(xor(hamming_code(1), hamming_code(3)), xor(hamming_code(5), hamming_code(7)));
r2 = xor(xor(hamming_code(2), hamming_code(3)), xor(hamming_code(6), hamming_code(7)));
r3 = xor(xor(hamming_code(4), hamming_code(5)), xor(hamming_code(6), hamming_code(7)));

% Determine the position of the error (if any)
error_position = r1 + 2*r2 + 4*r3;

if error_position == 0
    disp('No errors detected.');
else
    disp(['Error detected at bit position: ' num2str(error_position)]);
    % Correct the error
    hamming_code(error_position) = ~hamming_code(error_position);
    disp('Corrected Hamming Code: ');
    disp(hamming_code);
end
