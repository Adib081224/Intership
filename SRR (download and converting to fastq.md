import os
import subprocess
import time

# List of SRR IDs to download
srr_ids = [
    "SRR30230879",
    "SRR30230882",
    "SRR30230883",
    "SRR30230886",
    "SRR30230887"
]

# Set the full path to SRA Toolkit binaries with your specific path
sratoolkit_path = r"C:\Users\adibw\Desktop\computational Virology\sratoolkit.3.2.1-win64\bin"
prefetch_path = os.path.join(sratoolkit_path, "prefetch.exe")
fasterq_dump_path = os.path.join(sratoolkit_path, "fasterq-dump.exe")

def download_srr(srr_id, output_dir="./data"):
    """Download a single SRR file and convert to FASTQ"""
    # Create output directory if it doesn't exist
    os.makedirs(output_dir, exist_ok=True)
    
    print(f"Downloading {srr_id}...")
    try:
        # Run prefetch to download SRA file
        subprocess.run([prefetch_path, srr_id], check=True)
        
        # Convert to FASTQ - use the SRR ID directly, as prefetch downloads to the default location
        subprocess.run([fasterq_dump_path, "--split-files", "--outdir", output_dir, srr_id], check=True)
        
        print(f"Successfully downloaded {srr_id}")
        return True
    except Exception as e:
        print(f"Error downloading {srr_id}: {e}")
        return False

def main():
    output_dir = "./data"
    
    # Make sure output directory exists and is absolute path
    output_dir = os.path.abspath(output_dir)
    os.makedirs(output_dir, exist_ok=True)
    
    # Check if SRA Toolkit paths exist
    if not os.path.exists(prefetch_path):
        print(f"ERROR: Could not find prefetch at {prefetch_path}")
        print("Please ensure the sratoolkit_path variable is correct.")
        return
        
    if not os.path.exists(fasterq_dump_path):
        print(f"ERROR: Could not find fasterq-dump at {fasterq_dump_path}")
        print("Please ensure the sratoolkit_path variable is correct.")
        return
    
    print(f"Starting download of {len(srr_ids)} SRR files")
    print("-" * 50)
    
    success = 0
    failed = []
    
    start_time = time.time()
    
    for srr in srr_ids:
        if download_srr(srr, output_dir):
            success += 1
        else:
            failed.append(srr)
    
    end_time = time.time()
    
    print("-" * 50)
    print(f"Download Summary:")
    print(f"Total time: {end_time - start_time:.2f} seconds")
    print(f"Successful: {success}/{len(srr_ids)}")
    
    if failed:
        print(f"Failed: {len(failed)}/{len(srr_ids)}")
        print(f"Failed SRRs: {', '.join(failed)}")

if __name__ == "__main__":
    main()
