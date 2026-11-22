# Software Architecture Katas

**Exploring software architecture, system design, and engineering trade-offs through practical challenges.**

## Purpose

This repository collects my proposed solutions to architectural katas and system design challenges. For each exercise, I explain the reasoning behind the architecture, including the constraints that shaped it, the trade-offs involved, how the system might scale, and what could happen when parts of it fail.

I'm a fan of [code katas and their variants](https://blog.thesoftwarecraft.com/2014/02/code-katas.html) because they make room for deliberate practice. After more than a decade in software engineering, I use these exercises to keep my skills sharp and share how I work through design decisions, especially with people who are new to the field. I treat each design as a proposal that can evolve through discussion, experimentation, and feedback.

## What You'll Find

- Architecture diagrams and system design proposals
- State machines and event-driven workflows where they help explain the design
- Architecture Decision Records (ADRs) and Requests for Comments (RFCs), where applicable
- Scalability, reliability, and resilience considerations
- Open questions and alternative approaches

## Way of Work

I pick a kata at random from [Architectural Katas](https://www.architecturalkatas.com/), sketch out a solution in a diagramming tool (I've been using [draw.io](https://www.drawio.com/)), and document my architectural decisions in the appropriate formats.

I deliberately choose exercises in a range of domains, including ones outside banking and e-commerce, where I've delivered software professionally. Working through unfamiliar business problems helps me build on my experience and broaden the range of problems I can tackle.

## On the Use of AI

A kata is about practicing techniques until they become second nature. That's why I work through the design myself. In my day-to-day work, I see AI tools as an important way to deliver software faster.

Deliberate practice builds the judgment I need to ask AI tools better questions and evaluate their answers. That judgment helps me get value from these tools while avoiding technical debt and operational risks.

I use AI to clarify concepts, find better names for parts of a design, and review and challenge my proposals. I make the design decisions myself and stay selective about where AI fits into the exercise.

## Repository Structure

Each kata solution will have its own folder with a README containing the exercise prompt, a solution diagram in `.drawio` and `.jpeg` formats, and any supporting diagrams, RFCs, or ADRs that the exercise calls for.

I'll also share the templates I use and credit their sources.

## Contributing

Every architecture reflects a set of constraints. If you see a trade-off worth revisiting or want to explore a different approach, feel free to open an issue or submit a pull request. Questions, critiques, and alternative designs all help us learn from each other and become better engineers.

## References and Study Materials

These are some of the resources I've relied on to study system design and software architecture.

- Books
  - [Fundamentals of Software Architecture](https://www.oreilly.com/library/view/fundamentals-of-software/9781492043447/)
  - [System Design Interview](https://bytebytego.com/courses/system-design-interview/scale-from-zero-to-millions-of-users)
  - [Patterns of Enterprise Application Architecture](https://martinfowler.com/books/eaa.html)
- Blogs & Newsletters
  - [ByteByteGo](https://blog.bytebytego.com/)
  - [The Pragmatic Engineer](https://newsletter.pragmaticengineer.com/)
- Templates & Resources
  - [MADR — Markdown Architectural Decision Records](https://adr.github.io/madr/)
  - [Rust RFC Template](https://github.com/rust-lang/rfcs/blob/main/0000-template.md?utm_source=chatgpt.com)