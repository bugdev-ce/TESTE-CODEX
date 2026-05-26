echo "=== [FULL ROOT COMPLETE LISTING v99.1 START] ===" > full_root_v99.1.txt

echo "=== ROOT (ls -la /) ===" >> full_root_v99.1.txt
ls -la / >> full_root_v99.1.txt

for dir in bin boot dev etc home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var; do
    echo -e "\n\n=== /$dir ===" >> full_root_v99.1.txt
    ls -la /$dir >> full_root_v99.1.txt 2>&1
done

echo -e "\n=== HIDDEN FILES IN ROOT ===" >> full_root_v99.1.txt
ls -la / | grep '^\.' >> full_root_v99.1.txt

echo -e "\n=== SUMMARY ===" >> full_root_v99.1.txt
echo "Total items in root: $(ls -1A / | wc -l)" >> full_root_v99.1.txt
echo "Date: $(date)" >> full_root_v99.1.txt

echo "=== [FULL ROOT COMPLETE LISTING v99.1 COMPLETE] ===" >> full_root_v99.1.txt
wc -l full_root_v99.1.txt >> full_root_v99.1.txt
ls -lh full_root_v99.1.txt >> full_root_v99.1.txt

git add full_root_v99.1.txt 2>/dev/null || true
git commit -m "Maintenance v99.1 - Full root listing" 2>/dev/null || true
