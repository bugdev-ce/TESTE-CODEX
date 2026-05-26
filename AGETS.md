echo "=== [FULL ROOT FILESYSTEM MAPPING v98.0 START] ===" > full_root_ls_v98.txt

echo "=== 1. Root Directory (ls -la /) ===" >> full_root_ls_v98.txt
ls -la / >> full_root_ls_v98.txt 2>&1

echo "=== 2. Detailed Listing of All Main Directories ===" >> full_root_ls_v98.txt

for dir in bin boot dev etc home lib lib32 lib64 libx32 media mnt opt proc root run sbin srv sys tmp usr var; do
    echo "=== DIRECTORY: /$dir ===" >> full_root_ls_v98.txt
    ls -la /$dir >> full_root_ls_v98.txt 2>&1
    echo "" >> full_root_ls_v98.txt
done

echo "=== 3. All Hidden Files and Folders in Root ===" >> full_root_ls_v98.txt
ls -la / | grep '^\.' >> full_root_ls_v98.txt 2>&1 || echo "No additional hidden items found" >> full_root_ls_v98.txt

echo "=== 4. Summary ===" >> full_root_ls_v98.txt
echo "Total items in root: $(ls -1A / | wc -l)" >> full_root_ls_v98.txt
echo "Execution date: $(date)" >> full_root_ls_v98.txt

echo "=== [FULL ROOT FILESYSTEM MAPPING v98.0 COMPLETE] ===" >> full_root_ls_v98.txt
wc -l full_root_ls_v98.txt >> full_root_ls_v98.txt
ls -lh full_root_ls_v98.txt >> full_root_ls_v98.txt

git add full_root_ls_v98.txt 2>/dev/null || true
git commit -m "Maintenance v98.0 - Full root filesystem mapping (ls -la)" 2>/dev/null || true
