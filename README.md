# AWS DevOps Professional Study Wiki

This repository contains my personal study notes, technical documentation, and resources for the **AWS Certified DevOps Engineer - Professional (DOP-C02)** certification. 

The wiki is designed to be browsed using [Obsidian](https://obsidian.md/), leveraging wikilinks and structured markdown files to interconnect different AWS services and DevOps concepts.

## 🗂️ Structure

- `/AWS-DevOps-Professional`: The main directory containing detailed documentation on core AWS services relevant to the exam (e.g., CodePipeline, Lambda, CloudFormation).
  - `/aws-devops-exam-guide`: Reference materials and domain breakdowns from the official AWS exam guide.
  - `/support-recources-aws`: A directory containing "stub" files for AWS services that are referenced in the main notes but haven't been fully documented yet.
  - `update.md`: A transient file used to draft new notes. An automated workflow processes this file, translates the content to technical English, and distributes it to the appropriate markdown files.

## 🚀 AI Agent Workflow

1. Draft notes are written in Portuguese inside the `update.md` file.
2. An automated agent (`wiki-manager`) parses the `update.md` file.
3. The agent translates the notes to professional technical English, links references using Obsidian syntax `[[Service Name]]`, creates new files or updates existing ones, and manages the stubs.
4. The `update.md` is then cleared and the changes are automatically committed and pushed to this repository.

## 📝 Technologies & Tools

- **Obsidian**: Markdown knowledge base for structuring notes.
- **Git/GitHub**: Version control and remote backup.
- **AI Agent Automation**: Used for content refinement, translation, and repository management.

---
*Prepared for the AWS DOP-C02 exam.*
