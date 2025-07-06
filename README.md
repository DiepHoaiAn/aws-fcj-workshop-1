# 🎟️ Workshop: Build a Serverless Event Booking Web App with AWS Amplify

This is a **Hugo-based documentation site** for the workshop **"Xây dựng Ứng dụng Web Đặt Vé Sự Kiện với AWS Amplify"**, designed for students and developers who want to learn how to create full-stack, serverless web applications using **AWS Amplify**.

> ✅ This workshop is part of the **AWS First Cloud Journey Internship Program (FCJ 2025)**
> 📚 Language: Vietnamese (Tiếng Việt)


## 📌 Workshop Overview

In this hands-on lab, you'll learn how to:

* 🧹 Build a modern frontend using **Vue.js**
* 🔐 Add authentication with **Amazon Cognito**
* 📂 Store and query data with **Amazon DynamoDB** via **GraphQL API (AppSync)**
* ☁️ Use **AWS Amplify CLI** to generate and manage backend services
* 🛠️ Deploy and test a fully working cloud-based booking application


## 📂 Repository Structure

This repository hosts the **static site** source for the workshop using [Hugo](https://gohugo.io/) + GitHub Pages.
The content is structured into modular chapters:

```
content/
├── 1-introduction/
├── 2-prerequisite/
├── 3-connect/
├── 4-cleanup/
static/
└── images/
```


## 🚀 How to View Locally

To run the Hugo site on your machine:

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
cd YOUR_REPO_NAME
hugo server
```

Then open your browser at [http://localhost:1313](http://localhost:1313)


## 🌐 Live Site (GitHub Pages)

> 📌 Hosted on GitHub Pages at:
> `https://your-username.github.io/your-repo-name`

(Replace with actual link)


## 📚 Who is this for?

* Students joining the **AWS FCJ Internship**
* Newcomers to **serverless architectures**
* Developers exploring **Amplify + Vue + DynamoDB**
* Anyone interested in building real-world cloud-native web apps


## 📄 License

This workshop is educational material based on the **AWS Workshop Studio** sample project.
You are free to use and adapt it for personal and non-commercial learning purposes.


## 🙇‍♀️ Credits

Developed and documented by:
**Diệp Hoài An** – HCMUTE – AWS FCJ 2025
Email: [diephoaian2003@gmail.com](mailto:diephoaian2003@gmail.com)

---
# 🛠️ Xây dựng Ứng dụng Web Đặt Vé Sự Kiện với AWS Amplify

Đây là tài liệu hướng dẫn thực hành trong khuôn khổ workshop **"Xây dựng Ứng dụng Web Đặt Vé Sự Kiện với AWS Amplify"**, thuộc chương trình **AWS First Cloud Journey** dành cho sinh viên.

> 🎓 Người thực hiện: Diệp Hoài An – Đại học Sư phạm Kỹ thuật TP.HCM  
> 🗓️ Ngày hoàn thành workshop: 05/07/2025  
> 🌐 Giao diện tài liệu: Hugo + GitHub Pages (theme: Workshop)


## 📚 Nội dung chính

Workshop giúp bạn từng bước xây dựng một ứng dụng web hiện đại theo mô hình **serverless**, sử dụng các dịch vụ của AWS như:

- **Amazon Cognito** – xác thực người dùng
- **Amazon DynamoDB** – cơ sở dữ liệu NoSQL
- **AWS AppSync** – GraphQL API kết nối backend
- **AWS Amplify CLI** – công cụ triển khai và quản lý toàn bộ kiến trúc
- **Vue.js + Pinia** – giao diện frontend SPA

Tất cả thao tác đều thực hiện qua dòng lệnh và VS Code, mô phỏng vai trò của một kỹ sư phần mềm mới gia nhập dự án tại **FCJ-UTE Computing**.


## 🏗️ Cấu trúc tài liệu

Tài liệu được chia theo các chương tương ứng với các bước thực hiện trong workshop:

- `1. Giới thiệu & Kiến trúc hệ thống`
- `2. Thêm xác thực người dùng bằng Amazon Cognito`
- `3. Truy xuất và cập nhật dữ liệu bằng DynamoDB + GraphQL`
- `4. Kết luận & Xoá tài nguyên AWS`


## 🚀 Cách triển khai

1. Clone repo này về máy:
   ```bash
   git clone https://github.com/your-username/aws-ticket-workshop.git
   ```
2. Cài Hugo

```bash
# Dành cho macOS
brew install hugo

# Dành cho Windows
choco install hugo-extended -confirm
```
3. Chạy thử local
```bash
hugo serve
```
### Deploy lên GitHub Pages
- Đã cấu hình sẵn qua file .github/workflows/deploy.yml  
- Mỗi lần push vào nhánh main, hệ thống sẽ tự động build & publish trang web bằng GitHub Actions.  

📌 Ghi chú
- Đây là tài liệu học thuật, không dùng cho mục đích thương mại.  
- Mọi dữ liệu mock sử dụng trong ứng dụng MUSSEL là mô phỏng và công khai bởi đội ngũ phát triển AWS Studio
- Khi triển khai thật, cần xem xét kỹ về bảo mật, phân quyền và chi phí sử dụng dịch vụ.  

❤️ Cảm ơn
Tài liệu này được thực hiện trong chương trình AWS First Cloud Journey Internship, lấy cảm hứng từ workshop chính thức của AWS Workshop Studio.

👉 Chúc bạn học tập hiệu quả và thành công với AWS!

