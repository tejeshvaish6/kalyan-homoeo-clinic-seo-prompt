# Universal SEO Implementation Prompt for Any Website

## Overview
This comprehensive prompt provides a complete SEO implementation guide that can be applied to any website using React, TypeScript, and Vite. It covers technical SEO, content optimization, structured data, performance enhancements, and monitoring strategies.

## 1. Core SEO Dependencies

### Required Packages
```json
{
  "react-helmet-async": "^1.3.0",
  "vite-plugin-compression": "^0.5.1"
}
```

### Installation Commands
```bash
npm install react-helmet-async
npm install vite-plugin-compression --save-dev
```

## 2. Universal SEO Component

### Create `src/components/Seo.tsx`
```typescript
import { Helmet } from "react-helmet-async";
import React from "react";

interface SeoProps {
  title: string;
  description: string;
  canonical: string;
  image?: string;
  /**
   * JSON-LD structured data. Can be a single object or an array/@graph.
   */
  schema?: Record<string, unknown> | Record<string, unknown>[];
  /**
   * Comma-separated keywords for the <meta name="keywords"> tag.
   */
  keywords?: string;
  /** e.g., "en_US" */
  locale?: string;
  /** Your website name */
  siteName?: string;
  /** Content author */
  author?: string;
  /** Twitter handle */
  twitterHandle?: string;
  robots?: string; // e.g., "index,follow"
  /** Additional meta tags */
  additionalMeta?: Array<{name?: string; property?: string; content: string}>;
}

/**
 * Universal SEO component that injects meta tags, Open-Graph, Twitter cards, and JSON-LD.
 * Customize the default values for your specific website.
 */
const Seo: React.FC<SeoProps> = ({
  title,
  description,
  canonical,
  image,
  schema,
  keywords,
  locale = "en_US",
  siteName = "Your Website Name",
  author = "Your Name",
  twitterHandle = "@yourhandle",
  robots = "index,follow",
  additionalMeta = [],
}) => {
  return (
    <Helmet htmlAttributes={{ lang: locale.split("_")[0] }}>
      {/* Primary Meta */}
      <title>{title}</title>
      <meta name="description" content={description} />
      {keywords && <meta name="keywords" content={keywords} />}
      <meta name="robots" content={robots} />
      <meta name="author" content={author} />
      <link rel="canonical" href={canonical} />

      {/* Open Graph */}
      <meta property="og:title" content={title} />
      <meta property="og:description" content={description} />
      <meta property="og:type" content="website" />
      <meta property="og:url" content={canonical} />
      {image && <meta property="og:image" content={image} />}
      <meta property="og:site_name" content={siteName} />
      <meta property="og:locale" content={locale} />

      {/* Twitter */}
      <meta name="twitter:card" content="summary_large_image" />
      <meta name="twitter:title" content={title} />
      <meta name="twitter:description" content={description} />
      {image && <meta name="twitter:image" content={image} />}
      <meta name="twitter:site" content={twitterHandle} />

      {/* Additional Meta Tags */}
      {additionalMeta.map((meta, index) => (
        <meta
          key={index}
          {...(meta.name ? { name: meta.name } : {})}
          {...(meta.property ? { property: meta.property } : {})}
          content={meta.content}
        />
      ))}

      {/* JSON-LD Structured Data */}
      {schema && (
        <script type="application/ld+json">{JSON.stringify(schema)}</script>
      )}
    </Helmet>
  );
};

export default Seo;
```

## 3. Breadcrumb Component for Navigation SEO

### Create `src/components/Breadcrumb.tsx`
```typescript
import React from "react";
import { ChevronRight, Home } from "lucide-react";
import { Link } from "react-router-dom";

interface BreadcrumbItem {
  label: string;
  href?: string;
}

interface BreadcrumbProps {
  items: BreadcrumbItem[];
}

const Breadcrumb: React.FC<BreadcrumbProps> = ({ items }) => {
  return (
    <nav 
      className="flex items-center space-x-2 text-sm text-muted-foreground mb-6" 
      aria-label="Breadcrumb"
    >
      <Link 
        to="/" 
        className="flex items-center hover:text-primary transition-colors"
        aria-label="Home"
      >
        <Home className="h-4 w-4" />
        <span className="sr-only">Home</span>
      </Link>
      
      {items.map((item, index) => (
        <React.Fragment key={index}>
          <ChevronRight className="h-4 w-4" />
          {item.href ? (
            <Link 
              to={item.href} 
              className="hover:text-primary transition-colors"
              aria-current={index === items.length - 1 ? "page" : undefined}
            >
              {item.label}
            </Link>
          ) : (
            <span 
              className="text-foreground font-medium"
              aria-current="page"
            >
              {item.label}
            </span>
          )}
        </React.Fragment>
      ))}
    </nav>
  );
};

export default Breadcrumb;
```

## 4. HTML Head Optimization Template

### Update `index.html` with comprehensive meta tags
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Your Website Title | Your Brand Name</title>
    
    <!-- Favicon -->
    <link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />
    <link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png" />
    <link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png" />
    <link rel="manifest" href="/site.webmanifest" />
    <meta name="msapplication-TileColor" content="#ffffff" />
    <meta name="msapplication-TileImage" content="/favicon-32x32.png" />
    
    <!-- Primary Meta Tags -->
    <meta name="description" content="Your website description - keep it between 150-160 characters and include your primary keywords." />
    <meta name="author" content="Your Name" />
    <meta name="keywords" content="your, primary, keywords, separated, by, commas" />
    <meta name="robots" content="index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1" />
    <meta name="googlebot" content="index, follow" />
    <meta name="bingbot" content="index, follow" />
    <meta name="language" content="English" />
    
    <!-- Geographic Meta Tags (if applicable) -->
    <meta name="geo.region" content="US-CA" />
    <meta name="geo.placename" content="Your City" />
    <meta name="geo.position" content="latitude;longitude" />
    <meta name="ICBM" content="latitude, longitude" />
    <link rel="canonical" href="https://yourwebsite.com" />

    <!-- Performance Optimization -->
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link rel="preconnect" href="https://www.google-analytics.com" />
    <link rel="preconnect" href="https://www.googletagmanager.com" />
    <link rel="dns-prefetch" href="//fonts.googleapis.com" />
    <link rel="dns-prefetch" href="//www.google-analytics.com" />

    <!-- Mobile Optimization -->
    <meta name="format-detection" content="telephone=yes" />
    <meta name="theme-color" content="#ffffff" />
    <meta name="msapplication-navbutton-color" content="#ffffff" />
    <meta name="apple-mobile-web-app-status-bar-style" content="default" />
    <meta name="apple-mobile-web-app-capable" content="yes" />
    <meta name="apple-mobile-web-app-title" content="Your Website Name" />
    <meta name="application-name" content="Your Website Name" />
    <meta name="mobile-web-app-capable" content="yes" />
    <meta name="HandheldFriendly" content="true" />
    <meta name="MobileOptimized" content="width" />

    <!-- Open Graph Meta Tags -->
    <meta property="og:title" content="Your Website Title | Your Brand Name" />
    <meta property="og:description" content="Your website description for social media sharing." />
    <meta property="og:type" content="website" />
    <meta property="og:url" content="https://yourwebsite.com" />
    <meta property="og:site_name" content="Your Website Name" />
    <meta property="og:locale" content="en_US" />
    <meta property="og:image" content="https://yourwebsite.com/og-image.jpg" />
    <meta property="og:image:width" content="1200" />
    <meta property="og:image:height" content="630" />
    <meta property="og:image:alt" content="Your website description" />

    <!-- Twitter Card Meta Tags -->
    <meta name="twitter:card" content="summary_large_image" />
    <meta name="twitter:title" content="Your Website Title | Your Brand Name" />
    <meta name="twitter:description" content="Your website description for Twitter sharing." />
    <meta name="twitter:image" content="https://yourwebsite.com/og-image.jpg" />
    <meta name="twitter:site" content="@yourhandle" />
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
```

## 5. Vite Configuration for SEO Performance

### Update `vite.config.ts`
```typescript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react-swc";
import _compress from "vite-plugin-compression";
import path from "path";

const viteCompression = (typeof _compress === "function" ? _compress : (_compress as any).default) as unknown as (
  ...args: any[]
) => any;

export default defineConfig(({ mode }) => ({
  server: {
    host: "::",
    port: 8080,
  },
  plugins: [
    react(),
    viteCompression({ brotli: true }), // Enable compression for better performance
  ],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
          router: ['react-router-dom'],
        },
      },
    },
  },
}));
```

## 6. Universal Structured Data Templates

### Organization Schema (for Business Websites)
```typescript
const organizationSchema = {
  "@context": "https://schema.org",
  "@type": "Organization",
  name: "Your Organization Name",
  url: "https://yourwebsite.com",
  logo: "https://yourwebsite.com/logo.png",
  description: "Your organization description",
  address: {
    "@type": "PostalAddress",
    streetAddress: "Your Street Address",
    addressLocality: "Your City",
    addressRegion: "Your State/Region",
    postalCode: "Your Postal Code",
    addressCountry: "Your Country Code",
  },
  contactPoint: {
    "@type": "ContactPoint",
    telephone: "+1-XXX-XXX-XXXX",
    contactType: "customer service",
  },
  sameAs: [
    "https://www.facebook.com/yourpage",
    "https://www.twitter.com/yourhandle",
    "https://www.linkedin.com/company/yourcompany",
  ],
};
```

### Website Schema (for General Websites)
```typescript
const websiteSchema = {
  "@context": "https://schema.org",
  "@type": "WebSite",
  name: "Your Website Name",
  url: "https://yourwebsite.com",
  description: "Your website description",
  publisher: {
    "@type": "Organization",
    name: "Your Organization Name",
  },
  potentialAction: {
    "@type": "SearchAction",
    target: "https://yourwebsite.com/search?q={search_term_string}",
    "query-input": "required name=search_term_string",
  },
};
```

### Article Schema (for Blog Posts)
```typescript
const articleSchema = {
  "@context": "https://schema.org",
  "@type": "Article",
  headline: "Your Article Title",
  description: "Your article description",
  author: {
    "@type": "Person",
    name: "Author Name",
  },
  publisher: {
    "@type": "Organization",
    name: "Your Organization Name",
    logo: {
      "@type": "ImageObject",
      url: "https://yourwebsite.com/logo.png",
    },
  },
  datePublished: "2024-01-01",
  dateModified: "2024-01-01",
  mainEntityOfPage: {
    "@type": "WebPage",
    "@id": "https://yourwebsite.com/article-url",
  },
  image: "https://yourwebsite.com/article-image.jpg",
};
```

### FAQ Schema (for FAQ Pages)
```typescript
const faqSchema = {
  "@context": "https://schema.org",
  "@type": "FAQPage",
  mainEntity: [
    {
      "@type": "Question",
      name: "Your frequently asked question?",
      acceptedAnswer: {
        "@type": "Answer",
        text: "Your detailed answer to the question.",
      },
    },
    // Add more FAQ items as needed
  ],
};
```

### Product Schema (for E-commerce)
```typescript
const productSchema = {
  "@context": "https://schema.org",
  "@type": "Product",
  name: "Product Name",
  description: "Product description",
  image: "https://yourwebsite.com/product-image.jpg",
  brand: {
    "@type": "Brand",
    name: "Brand Name",
  },
  offers: {
    "@type": "Offer",
    price: "99.99",
    priceCurrency: "USD",
    availability: "https://schema.org/InStock",
  },
  aggregateRating: {
    "@type": "AggregateRating",
    ratingValue: "4.5",
    reviewCount: "100",
  },
};
```

## 7. Page-Specific SEO Implementation Examples

### Homepage Implementation
```typescript
import Seo from "@/components/Seo";

const HomePage = () => {
  return (
    <div className="min-h-screen">
      <Seo
        title="Your Website Title | Your Brand Name"
        description="Your homepage description with primary keywords. Keep it between 150-160 characters."
        canonical="https://yourwebsite.com/"
        keywords="your, primary, keywords, for, homepage"
        schema={organizationSchema}
        siteName="Your Website Name"
        author="Your Name"
        twitterHandle="@yourhandle"
      />
      {/* Rest of your homepage content */}
    </div>
  );
};
```

### Service/Product Page Implementation
```typescript
import Seo from "@/components/Seo";
import Breadcrumb from "@/components/Breadcrumb";

const ServicePage = () => {
  const breadcrumbItems = [
    { label: "Services", href: "/services" },
    { label: "Your Service Name" }
  ];

  const serviceSchema = {
    "@context": "https://schema.org",
    "@type": "Service",
    name: "Your Service Name",
    description: "Service description",
    provider: {
      "@type": "Organization",
      name: "Your Organization Name",
    },
  };

  return (
    <div className="min-h-screen container mx-auto px-4 py-16">
      <Seo
        title="Your Service Name | Your Brand Name"
        description="Service description with relevant keywords. Include location if applicable."
        canonical="https://yourwebsite.com/service-name"
        schema={serviceSchema}
      />
      
      <Breadcrumb items={breadcrumbItems} />
      
      <h1 className="text-3xl md:text-4xl font-bold mb-6">Your Service Name</h1>
      {/* Service content */}
    </div>
  );
};
```

### Blog Post Implementation
```typescript
import Seo from "@/components/Seo";
import Breadcrumb from "@/components/Breadcrumb";

const BlogPost = () => {
  const breadcrumbItems = [
    { label: "Blog", href: "/blog" },
    { label: "Your Blog Post Title" }
  ];

  return (
    <div className="min-h-screen container mx-auto px-4 py-16">
      <Seo
        title="Your Blog Post Title | Your Brand Name"
        description="Blog post description with relevant keywords. Include a call-to-action."
        canonical="https://yourwebsite.com/blog/post-title"
        schema={articleSchema}
      />
      
      <Breadcrumb items={breadcrumbItems} />
      
      <article className="max-w-4xl mx-auto">
        <header className="mb-8">
          <h1 className="text-4xl md:text-5xl font-bold mb-4">
            Your Blog Post Title
          </h1>
          <div className="flex items-center gap-4 text-sm text-muted-foreground mb-6">
            <span>By Author Name</span>
            <span>•</span>
            <time dateTime="2024-01-01">January 1, 2024</time>
            <span>•</span>
            <span>5 min read</span>
          </div>
        </header>
        {/* Article content */}
      </article>
    </div>
  );
};
```

## 8. SEO Files Setup

### Create `public/robots.txt`
```
User-agent: *
Allow: /

# Sitemap
Sitemap: https://yourwebsite.com/sitemap.xml

# Crawl-delay for respectful crawling
Crawl-delay: 1

# Allow all major search engines
User-agent: Googlebot
Allow: /
Crawl-delay: 1

User-agent: Bingbot
Allow: /
Crawl-delay: 1

User-agent: Slurp
Allow: /
Crawl-delay: 1

# Social media crawlers
User-agent: Twitterbot
Allow: /

User-agent: facebookexternalhit
Allow: /

User-agent: LinkedInBot
Allow: /

# Block unnecessary crawlers (optional)
User-agent: AhrefsBot
Disallow: /

User-agent: MJ12bot
Disallow: /

User-agent: DotBot
Disallow: /
```

### Create `public/sitemap.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9" xmlns:image="http://www.google.com/schemas/sitemap-image/1.1">
  <url>
    <loc>https://yourwebsite.com/</loc>
    <lastmod>2024-01-01</lastmod>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
    <image:image>
      <image:loc>https://yourwebsite.com/og-image.jpg</image:loc>
      <image:title>Your Website Title</image:title>
      <image:caption>Your website description</image:caption>
    </image:image>
  </url>
  <url>
    <loc>https://yourwebsite.com/about</loc>
    <lastmod>2024-01-01</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://yourwebsite.com/services</loc>
    <lastmod>2024-01-01</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.9</priority>
  </url>
  <url>
    <loc>https://yourwebsite.com/contact</loc>
    <lastmod>2024-01-01</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.7</priority>
  </url>
  <!-- Add more URLs as needed -->
</urlset>
```

### Create `public/site.webmanifest`
```json
{
  "name": "Your Website Name",
  "short_name": "Your Short Name",
  "description": "Your website description",
  "icons": [
    {
      "src": "/favicon-16x16.png",
      "sizes": "16x16",
      "type": "image/png"
    },
    {
      "src": "/favicon-32x32.png",
      "sizes": "32x32",
      "type": "image/png"
    },
    {
      "src": "/apple-touch-icon.png",
      "sizes": "180x180",
      "type": "image/png"
    },
    {
      "src": "/logo.png",
      "sizes": "192x192",
      "type": "image/png"
    }
  ],
  "theme_color": "#ffffff",
  "background_color": "#ffffff",
  "display": "standalone",
  "start_url": "/"
}
```

## 9. App.tsx Setup for Helmet Provider

### Update `src/App.tsx`
```typescript
import { HelmetProvider } from 'react-helmet-async';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
// Import your components

function App() {
  return (
    <HelmetProvider>
      <Router>
        <Routes>
          <Route path="/" element={<HomePage />} />
          <Route path="/about" element={<AboutPage />} />
          <Route path="/services" element={<ServicesPage />} />
          <Route path="/services/:serviceId" element={<ServicePage />} />
          <Route path="/blog" element={<BlogPage />} />
          <Route path="/blog/:postId" element={<BlogPost />} />
          <Route path="/contact" element={<ContactPage />} />
          <Route path="/products" element={<ProductsPage />} />
          <Route path="/products/:productId" element={<ProductPage />} />
          {/* Add more routes as needed */}
        </Routes>
      </Router>
    </HelmetProvider>
  );
}

export default App;
```

## 10. Content Optimization Guidelines

### Title Tag Optimization
- Include primary keyword + brand name
- Keep under 60 characters
- Use pipe (|) separator
- Example: "Best Web Design Services | Your Company Name"

### Meta Description Optimization
- Include primary keyword and location (if applicable)
- Add call-to-action
- Keep between 150-160 characters
- Example: "Professional web design services that boost your online presence. Custom websites, mobile-responsive design, and SEO optimization. Get your free quote today!"

### Keyword Strategy
- **Primary**: Main keyword for your business/service
- **Secondary**: Related keywords and variations
- **Long-tail**: Specific phrases your audience searches for
- **Local**: Include location-based keywords if applicable

### Content Structure
- Use H1 for main page title (only one per page)
- Use H2 for main sections
- Use H3 for subsections
- Include FAQ sections with structured data
- Add internal linking between related pages
- Include contact information and CTAs

## 11. Performance Optimization

### Image Optimization
- Use WebP format when possible
- Implement lazy loading
- Add proper alt tags
- Optimize file sizes
- Use responsive images

### Code Splitting
- Implement route-based code splitting
- Use dynamic imports for heavy components
- Optimize bundle size
- Remove unused code

### Caching Strategy
- Implement proper cache headers
- Use CDN for static assets
- Enable compression (Brotli/Gzip)
- Optimize database queries

## 12. Local SEO Implementation (if applicable)

### NAP Consistency
- **Name**: Your business name
- **Address**: Complete business address
- **Phone**: Business phone number

### Google My Business Optimization
- Complete profile with photos
- Regular posts and updates
- Encourage customer reviews
- Respond to reviews professionally
- Add business hours and services

### Local Keywords
- "business type near me"
- "service + location"
- "local business + city name"

## 13. E-commerce SEO (if applicable)

### Product Optimization
- Unique product titles with keywords
- Detailed product descriptions
- High-quality product images
- Customer reviews and ratings
- Product schema markup

### Category Pages
- Optimized category titles
- Category descriptions
- Filtered navigation
- Breadcrumb navigation

## 14. Monitoring and Analytics

### Essential Tools
- Google Search Console
- Google Analytics 4
- Google My Business Insights (if applicable)
- PageSpeed Insights
- Mobile-Friendly Test

### Key Metrics to Track
- Organic traffic growth
- Keyword rankings
- Click-through rates
- Page load speeds
- Mobile usability scores
- Conversion rates
- Bounce rate
- Session duration

## 15. Implementation Checklist

### Phase 1: Core Setup
- [ ] Install required dependencies
- [ ] Create SEO component
- [ ] Create Breadcrumb component
- [ ] Update HTML head with meta tags
- [ ] Configure Vite for performance

### Phase 2: Structured Data
- [ ] Implement homepage schema (Organization/Website)
- [ ] Add service/product page schemas
- [ ] Create blog post schemas (Article)
- [ ] Add FAQ schemas where applicable
- [ ] Implement product schemas (if e-commerce)

### Phase 3: Page Implementation
- [ ] Update homepage with SEO component
- [ ] Implement service/product pages
- [ ] Create blog pages
- [ ] Add about/contact pages
- [ ] Implement category pages (if applicable)

### Phase 4: SEO Files
- [ ] Create robots.txt
- [ ] Generate sitemap.xml
- [ ] Create site.webmanifest
- [ ] Add favicon files

### Phase 5: Content Optimization
- [ ] Optimize title tags
- [ ] Write compelling meta descriptions
- [ ] Implement keyword strategy
- [ ] Add internal linking
- [ ] Create FAQ content

### Phase 6: Performance & Monitoring
- [ ] Implement image optimization
- [ ] Set up analytics tracking
- [ ] Configure Google Search Console
- [ ] Monitor Core Web Vitals
- [ ] Track keyword rankings

## 16. Industry-Specific Adaptations

### For Service Businesses
- Focus on local SEO
- Include service area information
- Add service-specific schemas
- Implement review management

### For E-commerce
- Product schema implementation
- Category page optimization
- Shopping cart and checkout optimization
- Customer review integration

### For Blogs/Content Sites
- Article schema for all posts
- Category and tag optimization
- Author schema implementation
- Social sharing optimization

### For SaaS/Technology
- Feature-focused content
- Technical documentation SEO
- API documentation optimization
- Developer resource optimization

## 17. Advanced SEO Features

### Schema Markup Extensions
- Event schema (for events)
- Recipe schema (for food blogs)
- Course schema (for educational content)
- SoftwareApplication schema (for apps)

### Technical SEO
- Core Web Vitals optimization
- Mobile-first indexing
- HTTPS implementation
- URL structure optimization
- Internal linking strategy

### Content Strategy
- Keyword research and mapping
- Content calendar planning
- Topic cluster development
- User intent optimization

## Conclusion

This universal SEO implementation guide provides a comprehensive foundation for optimizing any website. The modular approach allows you to pick and choose the components that are relevant to your specific website type and industry.

Key success factors:
1. **Consistency**: Maintain consistent implementation across all pages
2. **Quality**: Focus on high-quality, user-focused content
3. **Performance**: Ensure fast loading times and mobile optimization
4. **Monitoring**: Regularly track and analyze SEO performance
5. **Adaptation**: Continuously adapt strategies based on data and industry changes

Remember to customize the default values, schemas, and content strategies based on your specific website, industry, and target audience. Regular monitoring and updates will help maintain and improve your SEO performance over time.
