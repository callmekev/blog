---
{"dg-publish":true,"permalink":"/tools/git-tool-py/","dg-note-properties":{}}
---

Git 可以做什麽？
* **版本控制與時光倒流**：記錄每一次的修改歷程，若程式碼改壞了，可隨時還原到過去任一正常版本。
* **分支與平行開發 (Branching)**：建立獨立的分支來測試新功能或實驗，確保主線（Main/Master）隨時保持穩定。
* **團隊協作與合併**：允許多位成員同時維護同一個專案，安全地整合彼此的程式碼並處理衝突。
* **變更追蹤與稽核**：透過 `diff` 清楚比對程式碼的增刪差異，並追溯每一行程式碼是誰在什麼時候修改的。
* **安全備份**：完整保留本機的歷史紀錄，並能推送到遠端雲端平台（如 GitHub、GitLab）雙重備份，防止硬碟故障導致心血全毀。


1 安裝git for windows

2 基本操作：

| 初始化      | git init                                                                  |
| -------- | ------------------------------------------------------------------------- |
| 查看專案狀態   | git status                                                                |
| 加入暫存區    | git add .                                                                 |
| 提交變更     | git commit -m "輸入你的提交說明訊息"                                                |
| git log  | 檢視過去所有的提交歷史紀錄。                                                            |
| git diff | 檔案被修改但尚未加入暫存區的具體內容差異<br><br>先把它加入 Git 的追蹤範圍（暫存區），之後如果再修改它，git diff 就會顯示差異 |

小脚本
```
import subprocess
import sys

def run_git_command(args):
    """執行 Git 指令並印出結果（指定 utf-8 編碼避免 Windows 編碼錯誤）"""
    try:
        result = subprocess.run(
            ['git'] + args, 
            capture_output=True, 
            encoding='utf-8', 
            errors='replace', 
            check=True
        )
        print(result.stdout)
        return True
    except subprocess.CalledProcessError as e:
        print(f"❌ Git 執行錯誤:\n{e.stderr}", file=sys.stderr)
        return False

def git_init():
    print("\n📌 【初始化 Git 專案】")
    run_git_command(['init'])

def git_status():
    print("\n📌 【查看專案狀態】")
    run_git_command(['status'])

def git_diff():
    print("\n📌 【查看程式碼差異 (diff)】")
    run_git_command(['diff'])

def git_add_commit():
    print("\n📌 【加入並提交變更】")
    message = input("請輸入 Commit 訊息: ").strip()
    if not message:
        print("⚠️ 訊息不能為空！")
        return
    if run_git_command(['add', '.']):
        run_git_command(['commit', '-m', message])

def git_log():
    print("\n📌 【查看最近提交歷史】")
    run_git_command(['log', '--oneline', '-n', '10'])

def main():
    while True:
        print("\n==============================")
        print("    本機專案 Git 自動化小工具    ")
        print("==============================")
        print("1. 初始化專案 (init)")
        print("2. 查看狀態 (status)")
        print("3. 查看修改差異 (diff)")
        print("4. 提交變更 (add & commit)")
        print("5. 查看歷史紀錄 (log)")
        print("0. 離開程式")
        
        choice = input("\n請選擇功能代號 (0-5): ").strip()
        
        if choice == '1':
            git_init()
        elif choice == '2':
            git_status()
        elif choice == '3':
            git_diff()
        elif choice == '4':
            git_add_commit()
        elif choice == '5':
            git_log()
        elif choice == '0':
            print("👋 離開程式。")
            break
        else:
            print("⚠️ 無效的選擇，請重新輸入。")

if __name__ == "__main__":
    main()
```



推送遠端：

| 遠端平臺建立空倉庫          |                                                                                                                                                                                                                           |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 將本機倉庫與遠端進行連接       | git remote add origin <你的遠端倉庫網址>                                                                                                                                                                                          |
| 設定預設分支名稱（通常為 main） | git branch -M main                                                                                                                                                                                                        |
| 首次推送至遠端            | git push -u origin main（彈出需要account 如github 的PAT）<br><br>加上 -u（--set-upstream）：<br><br>用途：第一次推送時使用，用來告訴 Git：「把本機的 main 分支和遠端的 origin/main 綁定在一起」。<br><br>好處：設定之後，以後你只要在該分支直接輸入 git push 或 git pull 即可，不用每次都打 origin main。 |
| 拉取遠端最新更新           | git pull origin main                                                                                                                                                                                                      |
|                    |                                                                                                                                                                                                                           |

