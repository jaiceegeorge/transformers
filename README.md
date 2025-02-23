import os
import numpy as np

def read_and_segment(file_path, segment_length=150, num_channels=5):
    """
    Reads data from a file, segments it into matrices, and returns the segments.

    Args:
        file_path: Path to the file containing the data.
        segment_length: Desired number of rows in each segment matrix.
        num_channels: Number of columns (channels) in each segment matrix.

    Returns:
        A NumPy array containing the segmented data.
    """

    all_values = []
    with open(file_path, 'r') as file:
        for line in file:
            values = line.strip().split()  # Split line by whitespace
            for value in values:
                try:
                    all_values.append(float(value))
                except ValueError:
                    pass  # Ignore non-numeric values

    # Reshape the data into a matrix
    num_segments = len(all_values) // (segment_length * num_channels)
    input_array = np.array(all_values[:num_segments * segment_length * num_channels]).reshape(num_segments, segment_length, num_channels)

    return input_array

# Directory containing the EMG data
emg_directories = ['/content/drive/My Drive/EMG/A_TXT', '/content/drive/My Drive/EMG/N_TXT']

all_segments = []
for emg_directory in emg_directories:
    for filename in os.listdir(emg_directory):
        if filename.endswith('.txt'):
            file_path = os.path.join(emg_directory, filename)
            input_array = read_and_segment(file_path)
            all_segments.append(input_array)


# Concatenate segments from all files
if all_segments:
    all_segments_array = np.concatenate(all_segments, axis=0)
    print(all_segments_array.shape) # Print the shape of the final array
    print(all_segments_array)
else:
    print("No .txt files found in the directory.")
