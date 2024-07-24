# Angular-Performance

To improve web performance, especially for enhancing user experience, three key metrics are often focused on: Largest Contentful Paint (LCP), First Input Delay (FID), and Cumulative Layout Shift (CLS). Here's an introduction to each:

## Largest Contentful Paint (LCP)
### Definition
LCP measures the time it takes for the largest content element visible within the viewport to load. This element is typically an image, video, or large block of text.

### Why It Matters
LCP is a critical metric for assessing perceived load speed. Users perceive a page to be loading quickly if the main content is rendered promptly.

### Tactics in Angular
#### 1. Lazy Loading Modules
Load only the necessary modules initially and defer loading of other modules until needed.
```typescript
const routes: Routes = [
  { path: '', loadChildren: () => import('./home/home.module').then(m => m.HomeModule) },
  { path: 'about', loadChildren: () => import('./about/about.module').then(m => m.AboutModule) },
];
```
#### 2. Optimize Images
Use Angular's built-in support for lazy loading images.
```html
<img [attr.loading]="lazy" src="image.png" alt="Optimized Image">
```
#### 3. Use Angular Universal for Server-Side Rendering (SSR)
Pre-render pages on the server to deliver content faster.
```
ng add @nguniversal/express-engine
```
#### 4. Preload Important Resources
Preload key resources like fonts and CSS to ensure they are available as soon as possible.
```html
<link rel="preload" href="styles.css" as="style">
```
#### 5. Optimize Angular Change Detection
Use OnPush change detection strategy to reduce unnecessary checks.
```typescript
@Component({
  selector: 'app-component',
  templateUrl: './component.html',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class AppComponent {}
```
## Improving First Input Delay (FID)
### Definition
FID measures the time from when a user first interacts with your page (e.g., clicks a link, taps a button) to the time when the browser begins processing that interaction.

### Why It Matters
FID is crucial for interactivity. A low FID ensures that the page responds quickly to user inputs, which is essential for a smooth user experience.

### Tactics in Angular
#### 1. Code Splitting
Break down the application into smaller bundles to reduce the load time and improve interactivity.
```typescript
const routes: Routes = [
  { path: 'feature', loadChildren: () => import('./feature/feature.module').then(m => m.FeatureModule) }
];
```
#### 2. Optimize Event Listeners
Avoid adding too many event listeners and debounce high-frequency events.
```typescript
@HostListener('window:scroll', ['$event'])
onScroll(event) {
  this.debounce(this.handleScroll, 200);
}

debounce(func, wait) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}

handleScroll(event) {
  // Handle scroll event
}
```
#### 3. Defer Non-Critical JavaScript
Use Angular's built-in tools to defer non-essential scripts.
```html
<script src="non-critical.js" defer></script>
```
#### 4. Use Web Workers
Offload heavy computations to web workers.
```typescript
const worker = new Worker('./path/to/worker.js', { type: 'module' });
worker.postMessage({ data: 'some data' });
```

## Improving Cumulative Layout Shift (CLS)
### Definition
CLS measures the total amount of unexpected layout shifts that occur during the entire lifespan of the page. It is a score based on how much visible content shifts and the movement distance relative to the viewport.

### Why It Matters
CLS is essential for visual stability. Unexpected shifts can cause a poor user experience, especially if users are interacting with the page and elements move unexpectedly.

### Tactics in Angular
#### 1. Set Explicit Sizes for Images and Videos
Ensure all media elements have defined width and height.
```html
<img src="image.jpg" width="600" height="400" alt="Example Image">
```
#### 2. Reserve Space for Dynamic Content
Pre-allocate space for dynamic content such as ads or async-loaded components.
```html
<div style="width: 300px; height: 250px;">
  <!-- Ad content here -->
</div>
```
#### 3. Load Fonts Efficiently
Use font-display: swap in your CSS to avoid invisible text during font loading.
```css
@font-face {
  font-family: 'CustomFont';
  src: url('custom-font.woff2') format('woff2');
  font-display: swap;
}
```
#### 4. Avoid Injecting Content Above Existing Content
When dynamically inserting content, avoid placing it above existing content unless absolutely necessary.
