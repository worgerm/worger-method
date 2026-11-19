# Miscellaneous Standards
This is where uncategorised standards are captured.

## Use Git for source control
Use Git for new repositories, and migrate existing repositories to Git if you're going to make anything more than very minor changes

**Why:** Git is ubiquitous and capable enough for the vast majority of needs

## Do not commit binaries to source control
Never commit binaries directly to source control. Store first party binaries (e.g. images) in source control extensions (e.g. Git LFS), or artefact repositories.

**Why:** Source control generally doesn't diff binaries, so any change stores the whole binary again, regardless of how much of it has changed. Fast cloning of source repositories is increasingly important as we move to ephemeral infrastructure, which is significantly slowed by directly storing binaries in source control.

## Pull dependencies from artefact repositories
Avoid manual vendoring dependencies whenever practical, especially manual vendoring. If you must retain control of a copy, then prefer doing so as an independent entity outside your first party source.

**Why:** Many build tools are specifically designed to manage source and binary dependencies, and will do so in a much more controlled and standardised way than manual vendoring. Some build tools support automated vendoring, beware of licensing.

## Do not commit generated source
Never commit generated source. Generated source should only be persisted in outputs, whether they are compiled or not.

**Why:** If generated source is committed then at some point someone will manually modify it, and from then on it will lose all of the benefits of being generated.

## No build logic in CI/CD infrastructure
Do not write or configure any build logic into CI/CD infrastructure e.g. never write shell scripts in a CI/CD build configuration, even if it's managed with configuration as code. CI/CD configuration must only ever call other build tools e.g. gradle, maven, dotnet

**Why:** Anything that you can only do in your CI/CD infrastructure introduces long round trips whenever you make changes. You must always be able to perform all build operations locally without requiring your CI/CD infrastructure.

## Use Configuration as Code everywhere
Use Configuration as Code everywhere it is sufficiently supported. Manual configuration of CI/CD builds, build environments etc. should never be required. Be careful with security, Configuration as Code is more powerful than you realise and often all it requires to leverage is commit rights on a repository.

**Why:** Manual configuration is prone to mistakes, time-consuming to both perform and document, poorly auditable, poorly scalable

## Select programming languages that guarantee robustness
Prefer programming languages that are statically typed. If a dynamically typed language is selected, configure it to behave as statically as possible.

Avoid writing manual memory management i.e. manual allocation and releasing of memory.
- Garbage collection is simplest, requiring very little effort and greatly limits issues, but at the expense of unpredicable release timing and a little memory overhead.
- RAII / Smart Pointers / Borrow Checking still requires focussed design, but is much more robust than manual management and can extend into threading safety in the case of Rust.

**Why:**
- There are classes of errors that statically typed programming languages catch at build time, earlier in the lifecycle and hence cheaper to fix. Statically typed code reduces cognitive load.
- Manual memory management is complex, and one of the most common causes of bugs. Don't write it yourself unless you really have to.

## Decision-making effort proportional to the cost of changing it
Spend the most amount of time on decisions that take the longest or cost the most to change.

**Why:** If a decision can be quickly and cheaply changed, then there is little value in spending time upfront to make the best decision with imperfect information.

## Use BDD
Use BDD processes and technologies for all levels of abstraction higher than algorithmic unit tests.

**Why:** The BDD process drives alignment across stakeholders, at the level of detail that matters to them, and in a formal that they can understand. BDD technologies combine the values provided by requirements and tests without duplicating effort by producing both.

## Near full automated test coverage
In software, there is very little that can't be tested using automated tests. Design for automated testing from the start in greenfield work, and aggressively drive automated testing for any legacy that doesn't already have it.

**Why:** 