# NANDPAL V3 - Complete System Design Document

## 📱 Project Overview

**NANDPAL V3** is a comprehensive AI-powered spiritual and educational mobile application built with React Native/Expo and Node.js. The app combines Krishna consciousness teachings with gamification, interactive features, and AI-driven engagement to create an immersive spiritual learning platform for users of all ages.

**Key Theme**: Krishna Consciousness with divine aesthetics and spiritual wisdom teachings

## 🏗️ System Architecture

### High-Level Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Mobile App    │    │   Web Dashboard  │    │  Admin Panel    │
│  (React Native) │    │    (React.js)    │    │   (React.js)    │
└─────────────────┘    └──────────────────┘    └─────────────────┘
         │                        │                        │
         └────────────────────────┼────────────────────────┘
                                  │
                    ┌─────────────▼─────────────┐
                    │      API Gateway          │
                    │    (Express.js + CORS)    │
                    └─────────────┬─────────────┘
                                  │
         ┌────────────────────────┼────────────────────────┐
         │                        │                        │
┌────────▼────────┐    ┌─────────▼─────────┐    ┌─────────▼─────────┐
│  User Service   │    │   Content Service │    │    AI Service     │
│   (Node.js)     │    │    (Node.js)      │    │   (Gemini API)    │
└─────────────────┘    └───────────────────┘    └───────────────────┘
         │                        │                        │
┌────────▼────────┐    ┌─────────▼─────────┐    ┌─────────▼─────────┐
│   MongoDB       │    │   Redis Cache     │    │   Cloudinary      │
│   Atlas         │    │   (1hr TTL)       │    │   (Media CDN)     │
└─────────────────┘    └───────────────────┘    └───────────────────┘
```

## Technology Stack

### Frontend (Mobile App)
- **Framework**: React Native 0.79.5 with Expo 53.0.17
- **Language**: TypeScript 5.9.2
- **Routing**: Expo Router 5.1.3
- **Styling**: NativeWind 4.1.23 (Tailwind for React Native)
- **State Management**: React Hooks + AsyncStorage
- **Navigation**: React Navigation with Bottom Tabs
- **Media**: Expo AV (audio/video), React Native Voice
- **UI Components**: Custom component library with Krishna-inspired design system

### Backend Services
- **Runtime**: Node.js (v16.0.0+)
- **Framework**: Express.js 4.18.2
- **Language**: TypeScript 5.9.2
- **Authentication**: JWT + bcryptjs (30-day expiration)
- **Rate Limiting**: express-rate-limit (100 req/15min)
- **Security**: Helmet 7.0.0, CORS enabled
- **File Upload**: Multer for media handling

### Database Layer
- **Primary Database**: MongoDB 7.5.0 (via Mongoose)
- **Caching Layer**: Redis (via ioRedis 5.8.2, 1-hour TTL)
- **Search**: MongoDB text indexes
- **Analytics**: MongoDB aggregation pipelines

### AI & External Services
- **Primary AI**: Google Gemini API (Krishna conversations)
- **Fallback AI**: Replicate API (LLaMA 2)
- **Text-to-Speech**: ElevenLabs API (Krishna voice)
- **Speech Recognition**: @react-native-voice/voice
- **Media CDN**: Cloudinary (video/image hosting)
- **Language Detection**: franc 6.2.0

### Infrastructure & DevOps
- **Backend Hosting**: Render.com
- **Database**: MongoDB Atlas (cloud)
- **CDN**: Cloudinary global distribution
- **Mobile Deployment**: Expo EAS Build
- **Environment**: Production + Development configs

## Detailed Component Design

### 1. Mobile Application Architecture

#### App Structure
```
src/
├── components/           # Reusable UI components
│   ├── common/          # Generic components
│   ├── forms/           # Form-specific components
│   └── animations/      # Custom animations
├── screens/             # Screen components
│   ├── auth/           # Authentication screens
│   ├── home/           # Home dashboard
│   ├── moral-walk/     # Daily moral walk
│   ├── chat/           # Kanha chat interface
│   ├── village/        # Nandgaon village
│   ├── chanting/       # Meditation & chanting
│   └── library/        # Shastrarth content
├── services/           # API and business logic
├── store/              # Redux store configuration
├── utils/              # Helper functions
├── assets/             # Images, fonts, sounds
└── types/              # TypeScript definitions
```

#### Key Features Implementation

**Daily Moral Walk Screen**
- Animated progress indicators
- Interactive story cards
- Mission tracking with visual feedback
- Reflection input with voice-to-text
- Streak visualization with celebration animations

**Friend Kanha Chat**
- Real-time messaging interface
- Typing indicators and message status
- Voice message support
- Contextual quick replies
- Emotion detection from text input

**Nandgaon Village**
- 2D isometric village view
- Drag-and-drop building placement
- Smooth animations for construction
- Progress bars for building upgrades
- Social sharing of village screenshots

### 2. Backend Service Architecture

#### Microservices Design

**User Service**
```javascript
// User profile management
- Authentication & authorization
- Profile creation and updates
- Progress tracking
- Parental controls
- Privacy settings
```

**Content Service**
```javascript
// Content management and delivery
- Daily lesson generation
- Story and verse curation
- Media file serving
- Content personalization
- Multilingual support
```

**AI Service**
```python
# AI-powered features
- Kanha conversation engine
- Sentiment analysis
- Content recommendation
- Personalization algorithms
- Safety filtering
```

**Analytics Service**
```javascript
// User behavior and progress analytics
- Event tracking
- Progress metrics
- Engagement analytics
- Parent reports
- A/B testing framework
```

#### API Design Examples

**Daily Moral Walk API**
```typescript
// GET /api/v1/moral-walk/today
interface DailyLesson {
  id: string;
  date: string;
  title: string;
  story: {
    text: string;
    audioUrl?: string;
    imageUrl?: string;
  };
  verse: {
    sanskrit: string;
    translation: string;
    meaning: string;
  };
  mission: {
    description: string;
    difficulty: 'easy' | 'medium' | 'hard';
    estimatedTime: number;
  };
  reflection: {
    questions: string[];
    prompts: string[];
  };
}
```

**Kanha Chat API**
```typescript
// POST /api/v1/chat/kanha
interface ChatRequest {
  message: string;
  context?: {
    currentMood?: string;
    recentActivities?: string[];
    userAge: number;
  };
}

interface ChatResponse {
  response: string;
  emotion: 'supportive' | 'encouraging' | 'wise' | 'playful';
  suggestedActions?: string[];
  relatedContent?: ContentReference[];
}
```

### 3. AI System Design

#### Kanha AI Architecture

**Core Components**:
1. **Intent Recognition**: Classify user messages into categories
2. **Context Management**: Maintain conversation history and user state
3. **Response Generation**: Generate appropriate responses using LLM
4. **Safety Filter**: Ensure all responses are age-appropriate and safe
5. **Cultural Alignment**: Verify responses align with Bhagavad Gita teachings

**Prompt Engineering**:
```python
KANHA_SYSTEM_PROMPT = """
You are Kanha, a wise and compassionate AI friend based on Lord Krishna's teachings from the Bhagavad Gita. You help children and young adults with moral guidance and emotional support.

Core Principles:
- Always respond with wisdom from Bhagavad Gita
- Be age-appropriate and culturally sensitive
- Provide practical, actionable advice
- Never give medical, legal, or predictive advice
- Maintain a warm, supportive tone
- Use simple language for younger users

User Context: {user_context}
Conversation History: {conversation_history}
Current Message: {user_message}

Respond as Kanha would, with wisdom, compassion, and practical guidance.
"""
```

#### Content Recommendation Engine

**Algorithm Flow**:
1. **User Profiling**: Analyze user behavior and preferences
2. **Content Scoring**: Rate content based on relevance and engagement
3. **Diversity Injection**: Ensure variety in recommendations
4. **Cultural Filtering**: Prioritize culturally relevant content
5. **Age Appropriateness**: Filter content based on user age

### 4. Database Schema Design

#### User Collection (MongoDB)
```javascript
{
  _id: ObjectId,
  userId: String,
  profile: {
    name: String,
    age: Number,
    language: ['hindi', 'english'],
    parentEmail?: String,
    avatar: String,
    createdAt: Date
  },
  progress: {
    currentStreak: Number,
    longestStreak: Number,
    totalLessonsCompleted: Number,
    villageLevel: Number,
    totalPoints: Number,
    achievements: [String]
  },
  preferences: {
    notificationTime: String,
    difficulty: String,
    interests: [String]
  },
  parentalControls: {
    enabled: Boolean,
    restrictions: [String],
    reportFrequency: String
  }
}
```

#### Daily Lessons Collection
```javascript
{
  _id: ObjectId,
  lessonId: String,
  title: String,
  ageGroup: ['8-12', '13-17', '18-30'],
  difficulty: ['beginner', 'intermediate', 'advanced'],
  content: {
    story: {
      text: String,
      audioUrl: String,
      imageUrl: String,
      duration: Number
    },
    verse: {
      sanskrit: String,
      transliteration: String,
      translation: {
        hindi: String,
        english: String
      },
      meaning: String,
      reference: String
    },
    mission: {
      description: String,
      category: String,
      difficulty: String,
      estimatedTime: Number
    }
  },
  tags: [String],
  createdAt: Date,
  updatedAt: Date
}
```

### 5. User Interface Design

#### Design System

**Color Palette**:
- Primary: Warm gold (#F4A460) - representing divine wisdom
- Secondary: Peaceful blue (#4682B4) - representing tranquility
- Accent: Lotus pink (#FFB6C1) - representing purity
- Background: Cream (#FFF8DC) - representing serenity
- Text: Dark brown (#8B4513) - representing earthiness

**Typography**:
- Headers: Custom Sanskrit-inspired font
- Body: Inter (clean, readable)
- Accent: Devanagari font for Sanskrit text

**Component Library**:
```typescript
// Button Component
interface ButtonProps {
  variant: 'primary' | 'secondary' | 'outline';
  size: 'small' | 'medium' | 'large';
  icon?: IconName;
  loading?: boolean;
  disabled?: boolean;
}

// Card Component
interface CardProps {
  elevation: 'low' | 'medium' | 'high';
  padding: 'small' | 'medium' | 'large';
  borderRadius: 'small' | 'medium' | 'large';
  background?: 'default' | 'gradient' | 'image';
}
```

#### Screen Designs

**Home Screen Layout**:
```
┌─────────────────────────────────┐
│ Header: Welcome + Profile       │
├─────────────────────────────────┤
│ Daily Streak Progress Bar       │
├─────────────────────────────────┤
│ Featured Content Card           │
│ (Shastrarth highlight)          │
├─────────────────────────────────┤
│ Quick Actions Grid:             │
│ ┌──────┐ ┌──────┐              │
│ │Moral │ │Chat  │              │
│ │Walk  │ │Kanha │              │
│ └──────┘ └──────┘              │
├─────────────────────────────────┤
│ Bottom Navigation               │
└─────────────────────────────────┘
```

**Daily Moral Walk Screen**:
```
┌─────────────────────────────────┐
│ Progress: Day 4 of streak       │
├─────────────────────────────────┤
│ Story Card with Image           │
│ "Share Your Food with Friends"  │
├─────────────────────────────────┤
│ Bhagavad Gita Verse             │
│ (Sanskrit + Translation)        │
├─────────────────────────────────┤
│ Today's Mission Box             │
│ "Share lunch with a friend"     │
├─────────────────────────────────┤
│ Action Buttons:                 │
│ [Complete Mission] [Reflect]    │
└─────────────────────────────────┘
```

### 6. Security & Privacy Design

#### Data Protection
- **Encryption**: AES-256 for data at rest, TLS 1.3 for data in transit
- **Authentication**: Multi-factor authentication for parents
- **Authorization**: Role-based access control (RBAC)
- **Data Minimization**: Collect only necessary information
- **Anonymization**: Personal data anonymized in analytics

#### Child Safety Features
- **Content Filtering**: AI-powered inappropriate content detection
- **Conversation Monitoring**: Optional parental oversight of AI chats
- **Time Limits**: Configurable usage restrictions
- **Safe Mode**: Extra protection for younger users
- **Report System**: Easy reporting of concerning content

#### Privacy Compliance
- **COPPA Compliance**: Special protections for users under 13
- **GDPR Compliance**: Data portability and deletion rights
- **Local Data Laws**: Compliance with Indian data protection regulations
- **Parental Consent**: Verified consent for child accounts

### 7. Performance Optimization

#### Mobile App Performance
- **Code Splitting**: Lazy loading of screens and components
- **Image Optimization**: WebP format with multiple resolutions
- **Caching Strategy**: Aggressive caching of static content
- **Bundle Size**: Keep app size under 50MB
- **Battery Optimization**: Efficient background processing

#### Backend Performance
- **Database Indexing**: Optimized queries with proper indexes
- **Caching Layers**: Redis for frequently accessed data
- **CDN Integration**: Global content delivery network
- **Auto Scaling**: Kubernetes-based horizontal scaling
- **Load Balancing**: Intelligent traffic distribution

### 8. Monitoring & Analytics

#### Application Monitoring
- **Performance Metrics**: Response times, error rates, throughput
- **User Experience**: Crash reports, ANR detection, user flows
- **Business Metrics**: DAU, retention, engagement, conversion
- **Infrastructure**: Server health, database performance, costs

#### User Analytics
- **Engagement Tracking**: Feature usage, session duration, retention
- **Learning Progress**: Lesson completion, streak maintenance, growth
- **Content Performance**: Popular content, completion rates, feedback
- **A/B Testing**: Feature experiments and optimization

### 9. Deployment Architecture

#### Production Environment
```
┌─────────────────────────────────────────────────────────┐
│                    AWS Cloud Infrastructure             │
├─────────────────────────────────────────────────────────┤
│ Route 53 (DNS) → CloudFront (CDN) → ALB (Load Balancer)│
├─────────────────────────────────────────────────────────┤
│                    EKS Cluster                         │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐       │
│ │   Pod 1     │ │   Pod 2     │ │   Pod 3     │       │
│ │ User Service│ │Content Svc  │ │  AI Service │       │
│ └─────────────┘ └─────────────┘ └─────────────┘       │
├─────────────────────────────────────────────────────────┤
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐       │
│ │ MongoDB     │ │   Redis     │ │  Pinecone   │       │
│ │ Atlas       │ │  Cluster    │ │  Vector DB  │       │
│ └─────────────┘ └─────────────┘ └─────────────┘       │
└─────────────────────────────────────────────────────────┘
```

#### CI/CD Pipeline
1. **Code Commit**: Developer pushes to GitHub
2. **Automated Testing**: Unit, integration, and E2E tests
3. **Security Scanning**: SAST, DAST, and dependency checks
4. **Build Process**: Docker image creation and optimization
5. **Staging Deployment**: Automated deployment to staging
6. **Production Deployment**: Blue-green deployment strategy

### 10. Scalability Considerations

#### Horizontal Scaling
- **Microservices**: Independent scaling of services
- **Database Sharding**: User-based data partitioning
- **CDN Distribution**: Global content delivery
- **Auto-scaling**: Kubernetes HPA based on metrics

#### Performance Targets
- **Response Time**: < 200ms for API calls
- **Availability**: 99.9% uptime SLA
- **Concurrent Users**: Support 100K+ simultaneous users
- **Data Processing**: Handle 1M+ daily interactions

This comprehensive design ensures Nandpal can scale to serve millions of users while maintaining the highest standards of performance, security, and user experience. The architecture is built to grow with the platform's success while preserving the core values of safety, cultural authenticity, and moral guidance.