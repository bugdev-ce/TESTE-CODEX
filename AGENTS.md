echo "=== [APRIL 2024 FILES INVESTIGATION v101.0 START] ===" > april_files_investigation_v101.txt

echo "=== 1. Files modified around April 10 2024 ===" >> april_files_investigation_v101.txt
find /etc -type f -newermt "2024-04-01" ! -newermt "2024-04-15" -ls 2>/dev/null | head -n 30 >> april_files_investigation_v101.txt

echo -e "\n=== 2. Specific Files Mentioned (April 10) ===" >> april_files_investigation_v101.txt
for file in fstab networks host.conf resolv.conf hostname hosts; do
    echo -e "\n--- /etc/$file ---" >> april_files_investigation_v101.txt
    ls -l --time-style=long-iso /etc/$file 2>&1 >> april_files_investigation_v101.txt
    cat /etc/$file 2>&1 >> april_files_investigation_v101.txt
done

echo -e "\n=== 3. Files from April 8 2024 (e2scrub, mke2fs, etc) ===" >> april_files_investigation_v101.txt
for file in e2scrub.conf mke2fs.conf; do
    echo -e "\n--- /etc/$file ---" >> april_files_investigation_v101.txt
    ls -l --time-style=long-iso /etc/$file 2>&1 >> april_files_investigation_v101.txt
    cat /etc/$file 2>&1 >> april_files_investigation_v101.txt
done

echo -e "\n=== 4. Summary ===" >> april_files_investigation_v101.txt
echo "Oldest files in /etc from April 2024 investigated" >> april_files_investigation_v101.txt
echo "Date: $(date)" >> april_files_investigation_v101.txt

echo "=== [APRIL 2024 FILES INVESTIGATION v101.0 COMPLETE] ===" >> april_files_investigation_v101.txt
wc -l april_files_investigation_v101.txt >> april_files_investigation_v101.txt
ls -lh april_files_investigation_v101.txt >> april_files_investigation_v101.txt

git add april_files_investigation_v101.txt 2>/dev/null || true
git commit -m "Maintenance v101.0 - Investigation of April 2024 files in /etc" 2>/dev/null || true
