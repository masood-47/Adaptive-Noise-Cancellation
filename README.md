# Addaptive-Noise-Cancellation

% Parameters
fs = 3500;               % Sampling frequency (Hz)
t = 0:1/fs:1-1/fs;      % Time vector for signals

% Clean signal
f_clean = 50;           % Frequency of clean signal (Hz)
clean_signal = sin(2*pi*f_clean*t);

% Noisy signal
f_noise = 150;          % Frequency of noise signal (Hz)
noise_signal = sin(2*pi*f_noise*t);

% Noisy signal with added noise
snr = 10;               % Signal-to-Noise Ratio (dB)
noisy_signal = awgn(clean_signal + noise_signal, snr, 'measured');

% Adaptive Noise Cancellation
order = 10;             % Filter order
step_size = 0.01;       % LMS step size

% LMS Filter initialization
weights = zeros(1, order+1);
output_signal = zeros(size(noisy_signal));

% LMS Algorithm
for n = order+1:length(noisy_signal)
    input_signal = noisy_signal(n:-1:n-order);
    predicted_noise = weights * input_signal.';
    error = clean_signal(n) - predicted_noise;
    weights = weights + step_size * input_signal * error;
    output_signal(n) = error;
end

% Plotting the signals
figure;
subplot(3,1,1);
plot(t, clean_signal);
title('Clean Signal');
xlabel('Time (s)');
ylabel('Amplitude');

subplot(3,1,2);
plot(t, noisy_signal);
title('Noisy Signal');
xlabel('Time (s)');
ylabel('Amplitude');

subplot(3,1,3);
plot(t, output_signal);
title('Adaptive Noise Cancellation');
xlabel('Time (s)');
ylabel('Amplitude');

% Sound play of noisy signal
sound(noisy_signal, fs);

% Pause for the duration of the noisy signal playback
pause(1);

% Sound play of clean signal
sound(clean_signal, fs);

% Pause for the duration of the clean signal playback
pause(1);
