#!/bin/bash

# Define the base URL
base_url="http://175.45.176.68/ko/index.php?"

# Loop through dates from January 1, 2023, to July 15, 2024
start_date="2023-01-01"
end_date="2024-08-01"

current_date="$start_date"
while [ "$(date -d "$current_date" +%s)" -le "$(date -d "$end_date" +%s)" ]; do
    # Loop through N-001 to N-015 with leading zeros
    for ((suffix=1; suffix<=15; suffix++)); do
        # Format the date and suffix as needed (with leading zeros for suffix)
        formatted_string="15@$current_date-N$(printf "%03d" $suffix)@"

        # Base64 encode the formatted string
        encoded_string=$(echo -n "$formatted_string" | base64)

        # Construct the full URL
        full_url="${base_url}${encoded_string}"

        # Use curl to fetch the content (change options as needed)
        curl -s "$full_url"  # You might want to add -o <output_file> to save responses to files
    done

    # Move to the next date
    current_date=$(date -d "$current_date + 1 day" +%Y-%m-%d)
done
