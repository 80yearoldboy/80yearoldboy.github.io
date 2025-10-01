name: Generate Sitemap

on:
  push:
    branches: [ main, master ]
    paths: ['posts/**', 'pages/**']
  workflow_dispatch:  # 允许手动触发

jobs:
  generate-sitemap:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
      with:
        fetch-depth: 0  # 获取所有历史记录，用于检测文件变更
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.x'
    
    - name: Generate sitemap
      run: |
        python << 'EOF'
        import os
        import glob
        from urllib.parse import quote
        from datetime import datetime
        
        base_url = "https://80yearoldboy.github.io"
        directories = ['posts', 'pages']
        output_file = 'public/sitemap.txt'
        
        # 确保public目录存在
        os.makedirs('public', exist_ok=True)
        
        urls = []
        
        for directory in directories:
            if os.path.exists(directory):
                # 查找所有markdown和html文件
                for ext in ['**/*.md', '**/*.html', '**/*.markdown']:
                    pattern = os.path.join(directory, ext)
                    for file_path in glob.glob(pattern, recursive=True):
                        # 构建URL路径
                        if file_path.endswith('.md') or file_path.endswith('.markdown'):
                            # 对于markdown文件，移除扩展名
                            url_path = file_path.rsplit('.', 1)[0]
                        else:
                            url_path = file_path
                        
                        # 构建完整URL
                        full_url = f"{base_url}/{url_path}"
                        urls.append(full_url)
        
        # 写入sitemap文件
        with open(output_file, 'w') as f:
            for url in sorted(urls):
                f.write(url + '\n')
        
        print(f"Generated sitemap with {len(urls)} URLs")
        print("URLs found:")
        for url in sorted(urls):
            print(f"  - {url}")
        EOF
    
    - name: Commit and push if changed
      run: |
        git config --local user.email "action@github.com"
        git config --local user.name "GitHub Action"
        git add public/sitemap.txt
        git diff --staged --quiet || git commit -m "Auto-generate sitemap"
        git push
