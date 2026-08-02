---
type: check
layer: 1-universal
status: active
source: combined from anti-slop design-patterns.md + de-slop grading structure
---

# Check: design slop

Detect AI slop patterns in visual design and UX copy. Flag each instance with the specific element and a concrete suggestion. This check covers three layers: visual design, layout, and copy. A clean pass on all three is normal — only flag what is genuinely present.

## 1. Visual design slop

**Generic gradient backgrounds:**
- Purple/pink/blue mesh gradients (the "AI startup gradient")
- Holographic or iridescent gradients as primary design element
- Gradient overlays on every image or section
- Detection signal: if gradients appear on more than 2-3 elements without a content reason, flag it.

**Overused visual motifs:**
- Floating 3D geometric shapes (cubes, spheres, toruses) decorating sections
- Glassmorphism on components where it serves no functional purpose
- Neumorphism on elements that need clear affordance
- Particle systems or animated blobs with no narrative purpose
- Tilt/perspective transforms on cards without interaction intent
- Detection signal: if every section uses the same visual treatment, it is template-driven, not designed.

**Stock photo aesthetics:**
- Generic diverse workplace photos with no relation to the actual product
- Hero images of people enthusiastically pointing at screens
- Business handshakes, overhead MacBook + coffee shots
- Fix: ask what the actual product does and whether the image shows it.

**Color palette slop:**
- The canonical AI startup palette: `#7F5AF0` purple + `#2CB67D` cyan + `#FF6AC1` pink
- Full pastel palettes with no contrast
- Pure black (#000) and pure white (#FFF) as primary UI colors (not just accents)

**Typography slop:**
- Inter for every text element at every size
- Montserrat + Open Sans, Poppins + Roboto (the default pairing)
- Same font family for headings and body with only weight variation
- 5+ different font families in a single layout
- Display fonts used for body copy

## 2. Layout antipatterns

**Template-driven layouts:**
- Content forced into a layout that does not serve it
- Hero → Features grid → Testimonials → CTA — regardless of what the product actually is
- Every piece of content in a card, regardless of type

**Card overuse:**
- Using cards for content that does not need containment
- Cards within cards within cards
- Equal-sized cards for information of unequal importance

**Excessive whitespace without hierarchy:**
- Huge padding on every section that makes content feel lost
- Whitespace that implies breathing room but is actually emptiness
- No hierarchy established through spacing — everything equidistant

**Center-alignment of everything:**
- Long body copy center-aligned (harder to read past 3 lines)
- Center-aligned left-nav or left-anchored content
- Center alignment used as a substitute for hierarchy

## 3. UX copy slop

**"Empower your business" class headlines:**
- "Transform the way you work"
- "Unlock your potential"
- "Streamline your workflow"
- "The future of [category]"
- Fix: say what the product specifically does. "Outbound emails written and sent while you sleep" beats "AI-powered sales automation."

**Generic CTAs:**
- "Get Started" with no context
- "Learn More" linking to nothing specific
- "Sign Up Free" as the only primary action with no benefit stated
- Fix: the CTA should complete a specific sentence. "Start your first sequence" beats "Get Started."

**Buzzword-heavy feature descriptions:**
- "Leverage cutting-edge AI to drive synergistic outcomes"
- Features described by their technology, not their user benefit
- Fix: "Rewrites cold emails for each recipient's LinkedIn activity" beats "AI-powered personalization."

**Social proof slop:**
- Testimonials that say nothing specific: "This product changed everything for our team!"
- Fake-looking headshots with generic names
- Made-up statistics: "10x faster" with no methodology
- Logos of companies with no stated relationship to the product

## 4. What NOT to flag

Do not flag:
- Deliberate minimalism (sparse is not broken)
- Strong brand colors that happen to be vivid
- A single gradient used intentionally as a brand element
- Technical layouts (dashboards, tables) that are dense by necessity
- Clear, direct CTAs that happen to be short

The test: was this decision made for the content, or pulled from a template? If you cannot tell, it is not a finding.

## Grading

- Aligned: layout serves the content, visual treatment varies by element importance, copy is specific about what the product does.
- Drift: one or two template-y sections, a generic CTA, a buzzword headline — fixable without restructuring.
- Misaligned: gradient mesh background + floating 3D shapes + "Transform your workflow" headline + generic stock photos. The whole surface is template-output, not a design decision.
