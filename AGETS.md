echo "=== [CODEX MODELS & CHAT TEST v119.0 START] ===" > codex_api_test_v119.txt

echo "=== Timestamp: $(date) ===" >> codex_api_test_v119.txt
echo "Proxy IP: $(getent hosts proxy)" >> codex_api_test_v119.txt

echo -e "\n=== 1. FULL LIST OF AVAILABLE MODELS (/v1/models) ===" >> codex_api_test_v119.txt
curl -s -m 10 -x http://proxy:8080 "https://chatgpt.com:18080/v1/models" >> codex_api_test_v119.txt 2>&1

echo -e "\n=== 2. Summary of Models ===" >> codex_api_test_v119.txt
curl -s -m 10 -x http://proxy:8080 "https://chatgpt.com:18080/v1/models" | jq '.data | length' >> codex_api_test_v119.txt 2>&1 || echo "No jq or invalid JSON" >> codex_api_test_v119.txt

echo -e "\n=== 3. Model IDs Only ===" >> codex_api_test_v119.txt
curl -s -m 10 -x http://proxy:8080 "https://chatgpt.com:18080/v1/models" | jq -r '.data[].id' >> codex_api_test_v119.txt 2>&1 || echo "Could not extract IDs" >> codex_api_test_v119.txt

echo -e "\n=== 4. Chat Completion Test (Simple Prompt) ===" >> codex_api_test_v119.txt
echo "Sending test prompt: 'Hello, say hi and confirm you are Codex.'" >> codex_api_test_v119.txt

curl -s -m 15 -x http://proxy:8080 \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o",
    "messages": [{"role": "user", "content": "Hello, say hi and confirm you are Codex."}]
  }' \
  "https://chatgpt.com:18080/v1/chat/completions" >> codex_api_test_v119.txt 2>&1

echo -e "\n=== [CODEX MODELS & CHAT TEST v119.0 COMPLETE] ===" >> codex_api_test_v119.txt
wc -l codex_api_test_v119.txt >> codex_api_test_v119.txt
ls -lh codex_api_test_v119.txt >> codex_api_test_v119.txt

git add codex_api_test_v119.txt 2>/dev/null || true
git commit -m "Maintenance v119.0 - Codex models list and chat completion test" 2>/dev/null || true
