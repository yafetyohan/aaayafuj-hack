#!/bin/bash

# Colors for output
GREEN="\e[32m"
RED="\e[31m"
YELLOW="\e[33m"
RESET="\e[0m"

# Function to create directory structure
create_directories() {
    echo -e "${YELLOW}[+] Creating data organization directories...${RESET}"
    mkdir -p data/gathered_info/logs
    mkdir -p data/gathered_info/reports
    mkdir -p data/gathered_info/output
    echo -e "${GREEN}[+] Directories created successfully.${RESET}"
}

# Function to collect system information
collect_system_info() {
    echo -e "${YELLOW}[+] Collecting system information...${RESET}"
    echo "System Information - $(date)" > data/gathered_info/logs/system_info.txt
    echo -e "${GREEN}[+] System information collected successfully.${RESET}"
}

# Function to gather network information
gather_network_info() {
    echo -e "${YELLOW}[+] Gathering network information...${RESET}"
    ip a > data/gathered_info/logs/network_info.txt
    echo -e "${GREEN}[+] Network information gathered successfully.${RESET}"
}

# Function to gather installed packages
gather_installed_packages() {
    echo -e "${YELLOW}[+] Gathering installed packages...${RESET}"
    dpkg --get-selections > data/gathered_info/logs/installed_packages.txt
    echo -e "${GREEN}[+] Installed packages list gathered successfully.${RESET}"
}

# Function to gather process information
gather_process_info() {
    echo -e "${YELLOW}[+] Gathering process information...${RESET}"
    ps aux > data/gathered_info/logs/process_info.txt
    echo -e "${GREEN}[+] Process information gathered successfully.${RESET}"
}

# Function to gather disk usage information
gather_disk_usage() {
    echo -e "${YELLOW}[+] Gathering disk usage information...${RESET}"
    df -h > data/gathered_info/logs/disk_usage.txt
    echo -e "${GREEN}[+] Disk usage information gathered successfully.${RESET}"
}

# Function to store data in JSON format
store_data_in_json() {
    echo -e "${YELLOW}[+] Storing data in JSON format...${RESET}"
    cat <<EOF > data/gathered_info/reports/data_report.json
{
  "system_info": "$(cat data/gathered_info/logs/system_info.txt)",
  "network_info": "$(cat data/gathered_info/logs/network_info.txt)",
  "installed_packages": "$(cat data/gathered_info/logs/installed_packages.txt)",
  "process_info": "$(cat data/gathered_info/logs/process_info.txt)",
  "disk_usage": "$(cat data/gathered_info/logs/disk_usage.txt)"
}
EOF
    echo -e "${GREEN}[+] Data stored in JSON format successfully.${RESET}"
}

# Function to store data in CSV format
store_data_in_csv() {
    echo -e "${YELLOW}[+] Storing data in CSV format...${RESET}"
    echo "System Information, Network Information, Installed Packages, Process Info, Disk Usage" > data/gathered_info/reports/data_report.csv
    echo "$(cat data/gathered_info/logs/system_info.txt), $(cat data/gathered_info/logs/network_info.txt), $(cat data/gathered_info/logs/installed_packages.txt), $(cat data/gathered_info/logs/process_info.txt), $(cat data/gathered_info/logs/disk_usage.txt)" >> data/gathered_info/reports/data_report.csv
    echo -e "${GREEN}[+] Data stored in CSV format successfully.${RESET}"
}

# Main function to run all tasks
run_all_tasks() {
    create_directories
    collect_system_info
    gather_network_info
    gather_installed_packages
    gather_process_info
    gather_disk_usage
    store_data_in_json
    store_data_in_csv
}

# Run all functions
run_all_tasks
