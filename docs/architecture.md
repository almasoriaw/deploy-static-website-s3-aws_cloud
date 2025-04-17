# AWS Architecture: Static Website Hosting

This document outlines the AWS architecture used in this project to host a static website.

## Overview

The architecture uses two primary AWS services:
- **Amazon S3** for storage and static website hosting
- **Amazon CloudFront** for content delivery and global distribution

## Architecture Diagram

```
[ Web Client ] ---> [ CloudFront Distribution ] ---> [ S3 Bucket ]
```

## Components

### Amazon S3
- **Purpose**: Storage of static website files (HTML, CSS, JavaScript, images)
- **Configuration**:
  - Bucket with public read access
  - Static website hosting enabled
  - Policy allowing GetObject operations
- **Benefits**:
  - Highly available and durable storage
  - Pay only for what you use
  - Native static website hosting capabilities

### Amazon CloudFront
- **Purpose**: Content Delivery Network (CDN) for global distribution
- **Configuration**:
  - Origin set to S3 website endpoint
  - Default cache behaviors
  - HTTPS enabled
- **Benefits**:
  - Improved latency for global visitors
  - Edge caching reduces origin load
  - Built-in DDoS protection
  - HTTPS support with Amazon Certificate Manager
