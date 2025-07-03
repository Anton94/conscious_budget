# ConsciousBudget Project Structure Analysis

**Document Version:** 1.0  
**Created:** December 2024  
**Purpose:** Detailed project structure recommendation for cross-platform financial management application

## Executive Summary

Based on the ConsciousBudget Software Requirements Specification, this document provides a comprehensive project structure analysis optimized for cross-platform development with 80-90% code sharing between web, iOS, and Android platforms.

## Table of Contents

1. [Architecture Overview](#architecture-overview)
2. [Recommended Project Structure](#recommended-project-structure)
3. [Code Sharing Strategy](#code-sharing-strategy)
4. [Database Abstraction Architecture](#database-abstraction-architecture)
5. [Platform-Specific Considerations](#platform-specific-considerations)
6. [Development Workflow](#development-workflow)
7. [Implementation Phases](#implementation-phases)

---

## Architecture Overview

### Core Architectural Principles

1. **Monorepo Structure**: Single repository with multiple packages for maximum code sharing
2. **Offline-First**: Local data storage with optional cloud synchronization
3. **Serverless Backend**: No proprietary servers, leveraging Google Drive API
4. **Cross-Platform UI**: React Native with React Native Web for web deployment
5. **Layered Abstraction**: Clean separation between business logic, data access, and platform-specific code

### Technology Stack Summary

- **Frontend Framework**: React Native + React Native Web
- **Language**: TypeScript for type safety and better development experience
- **State Management**: Redux Toolkit with RTK Query for data fetching
- **Local Storage**: Platform-specific abstraction (AsyncStorage/IndexedDB)
- **Cloud Storage**: Google Drive API v3
- **Authentication**: Google OAuth 2.0
- **Charts/Visualization**: Victory Native (cross-platform) or react-chartjs-2
- **Navigation**: React Navigation v6
- **Testing**: Jest + React Native Testing Library

---

## Recommended Project Structure

```
ConsciousBudget/
├── package.json                 # Root workspace configuration
├── yarn.lock                   # Dependency lock file
├── .gitignore                  # Git ignore rules
├── .eslintrc.js                # ESLint configuration
├── .prettierrc                 # Prettier configuration
├── tsconfig.json               # TypeScript base configuration
├── jest.config.js              # Jest testing configuration
├── 
├── packages/                   # Shared packages
│   ├── shared-core/            # Core business logic (Platform-agnostic)
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   ├── src/
│   │   │   ├── types/          # TypeScript type definitions
│   │   │   │   ├── index.ts
│   │   │   │   ├── expense.types.ts
│   │   │   │   ├── budget.types.ts
│   │   │   │   ├── category.types.ts
│   │   │   │   ├── user.types.ts
│   │   │   │   └── sync.types.ts
│   │   │   ├── constants/      # Application constants
│   │   │   │   ├── index.ts
│   │   │   │   ├── categories.ts
│   │   │   │   ├── currencies.ts
│   │   │   │   └── config.ts
│   │   │   ├── utils/          # Pure utility functions
│   │   │   │   ├── index.ts
│   │   │   │   ├── date.utils.ts
│   │   │   │   ├── currency.utils.ts
│   │   │   │   ├── validation.utils.ts
│   │   │   │   ├── category.utils.ts
│   │   │   │   └── sync.utils.ts
│   │   │   ├── models/         # Data models and business logic
│   │   │   │   ├── index.ts
│   │   │   │   ├── Expense.ts
│   │   │   │   ├── Budget.ts
│   │   │   │   ├── Category.ts
│   │   │   │   ├── User.ts
│   │   │   │   └── RecurringTransaction.ts
│   │   │   └── validators/     # Data validation schemas
│   │   │       ├── index.ts
│   │   │       ├── expense.validators.ts
│   │   │       ├── budget.validators.ts
│   │   │       └── category.validators.ts
│   │   └── __tests__/          # Unit tests for core logic
│   │
│   ├── shared-ui/              # Shared UI components
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   ├── src/
│   │   │   ├── components/     # Reusable UI components (80% shared)
│   │   │   │   ├── index.ts
│   │   │   │   ├── forms/      # Form components
│   │   │   │   │   ├── ExpenseForm/
│   │   │   │   │   ├── BudgetForm/
│   │   │   │   │   ├── CategorySelector/
│   │   │   │   │   └── DatePicker/
│   │   │   │   ├── charts/     # Chart components
│   │   │   │   │   ├── PieChart/
│   │   │   │   │   ├── BarChart/
│   │   │   │   │   ├── LineChart/
│   │   │   │   │   └── SpendingTrends/
│   │   │   │   ├── lists/      # List components
│   │   │   │   │   ├── ExpenseList/
│   │   │   │   │   ├── BudgetList/
│   │   │   │   │   └── CategoryList/
│   │   │   │   ├── common/     # Basic UI components
│   │   │   │   │   ├── Button/
│   │   │   │   │   ├── Input/
│   │   │   │   │   ├── Card/
│   │   │   │   │   ├── Modal/
│   │   │   │   │   ├── LoadingSpinner/
│   │   │   │   │   └── ErrorBoundary/
│   │   │   │   └── layout/     # Layout components
│   │   │   │       ├── Screen/
│   │   │   │       ├── Header/
│   │   │   │       └── TabBar/
│   │   │   ├── hooks/          # Custom React hooks
│   │   │   │   ├── index.ts
│   │   │   │   ├── useExpenses.ts
│   │   │   │   ├── useBudgets.ts
│   │   │   │   ├── useCategories.ts
│   │   │   │   ├── useSync.ts
│   │   │   │   ├── useOfflineQueue.ts
│   │   │   │   └── useNotifications.ts
│   │   │   ├── styles/         # Shared styling
│   │   │   │   ├── index.ts
│   │   │   │   ├── colors.ts
│   │   │   │   ├── typography.ts
│   │   │   │   ├── spacing.ts
│   │   │   │   └── themes.ts
│   │   │   └── store/          # Redux store configuration
│   │   │       ├── index.ts
│   │   │       ├── store.ts
│   │   │       ├── slices/
│   │   │       │   ├── expensesSlice.ts
│   │   │       │   ├── budgetsSlice.ts
│   │   │       │   ├── categoriesSlice.ts
│   │   │       │   ├── userSlice.ts
│   │   │       │   ├── syncSlice.ts
│   │   │       │   └── settingsSlice.ts
│   │   │       └── middleware/
│   │   │           ├── syncMiddleware.ts
│   │   │           └── offlineMiddleware.ts
│   │   └── __tests__/          # Component tests
│   │
│   ├── shared-services/        # API and service layer
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   ├── src/
│   │   │   ├── storage/        # Storage abstraction layer
│   │   │   │   ├── index.ts
│   │   │   │   ├── StorageInterface.ts
│   │   │   │   ├── LocalStorage.ts      # Base local storage
│   │   │   │   ├── WebStorage.ts        # IndexedDB implementation
│   │   │   │   └── MobileStorage.ts     # AsyncStorage implementation
│   │   │   ├── cloud/          # Cloud storage services
│   │   │   │   ├── index.ts
│   │   │   │   ├── GoogleDriveService.ts
│   │   │   │   ├── GoogleAuthService.ts
│   │   │   │   └── GoogleSheetsService.ts
│   │   │   ├── sync/           # Synchronization services
│   │   │   │   ├── index.ts
│   │   │   │   ├── SyncManager.ts
│   │   │   │   ├── ConflictResolver.ts
│   │   │   │   └── OfflineQueue.ts
│   │   │   ├── backup/         # Backup and restore services
│   │   │   │   ├── index.ts
│   │   │   │   ├── BackupService.ts
│   │   │   │   └── RestoreService.ts
│   │   │   ├── notifications/  # Notification services
│   │   │   │   ├── index.ts
│   │   │   │   ├── NotificationService.ts
│   │   │   │   ├── WebNotifications.ts
│   │   │   │   └── MobileNotifications.ts
│   │   │   └── analytics/      # Analytics and logging
│   │   │       ├── index.ts
│   │   │       ├── AnalyticsService.ts
│   │   │       └── ErrorLogger.ts
│   │   └── __tests__/          # Service tests
│   │
│   └── shared-config/          # Shared configuration
│       ├── package.json
│       ├── src/
│       │   ├── index.ts
│       │   ├── environment.ts
│       │   ├── api.config.ts
│       │   ├── google.config.ts
│       │   └── platform.config.ts
│       └── __tests__/
│
├── apps/                       # Platform-specific applications
│   ├── mobile/                 # React Native mobile app
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   ├── metro.config.js
│   │   ├── babel.config.js
│   │   ├── react-native.config.js
│   │   ├── android/            # Android-specific files
│   │   │   ├── app/
│   │   │   ├── build.gradle
│   │   │   ├── gradle.properties
│   │   │   └── settings.gradle
│   │   ├── ios/                # iOS-specific files
│   │   │   ├── ConsciousBudget/
│   │   │   ├── ConsciousBudget.xcodeproj/
│   │   │   └── Podfile
│   │   ├── src/
│   │   │   ├── App.tsx         # Main app component
│   │   │   ├── components/     # Mobile-specific components
│   │   │   │   ├── navigation/ # Mobile navigation
│   │   │   │   │   ├── AppNavigator.tsx
│   │   │   │   │   ├── TabNavigator.tsx
│   │   │   │   │   └── StackNavigator.tsx
│   │   │   │   ├── screens/    # Mobile screen components
│   │   │   │   │   ├── HomeScreen/
│   │   │   │   │   ├── ExpenseEntryScreen/
│   │   │   │   │   ├── BudgetScreen/
│   │   │   │   │   ├── ReportsScreen/
│   │   │   │   │   ├── SettingsScreen/
│   │   │   │   │   └── AuthScreen/
│   │   │   │   └── platform/   # Platform-specific UI adaptations
│   │   │   │       ├── StatusBar/
│   │   │   │       ├── SafeArea/
│   │   │   │       └── Permissions/
│   │   │   ├── services/       # Mobile-specific services
│   │   │   │   ├── PushNotifications.ts
│   │   │   │   ├── BiometricAuth.ts
│   │   │   │   ├── FileSystem.ts
│   │   │   │   └── DeviceInfo.ts
│   │   │   └── utils/          # Mobile-specific utilities
│   │   │       ├── dimensions.ts
│   │   │       ├── permissions.ts
│   │   │       └── platform.ts
│   │   ├── __tests__/          # Mobile app tests
│   │   ├── e2e/               # End-to-end tests
│   │   └── .detoxrc.js        # Detox configuration
│   │
│   └── web/                    # React Native Web app
│       ├── package.json
│       ├── tsconfig.json
│       ├── webpack.config.js
│       ├── babel.config.js
│       ├── public/
│       │   ├── index.html
│       │   ├── manifest.json   # PWA manifest
│       │   ├── favicon.ico
│       │   └── icons/          # PWA icons
│       ├── src/
│       │   ├── App.tsx         # Main web app component
│       │   ├── components/     # Web-specific components
│       │   │   ├── navigation/ # Web navigation
│       │   │   │   ├── AppRouter.tsx
│       │   │   │   ├── Sidebar.tsx
│       │   │   │   └── TopNavigation.tsx
│       │   │   ├── pages/      # Web page components
│       │   │   │   ├── HomePage/
│       │   │   │   ├── DashboardPage/
│       │   │   │   ├── ExpensesPage/
│       │   │   │   ├── BudgetsPage/
│       │   │   │   ├── ReportsPage/
│       │   │   │   ├── SettingsPage/
│       │   │   │   └── AuthPage/
│       │   │   └── platform/   # Web-specific UI adaptations
│       │   │       ├── WebHeader/
│       │   │       ├── Responsive/
│       │   │       └── KeyboardShortcuts/
│       │   ├── services/       # Web-specific services
│       │   │   ├── ServiceWorker.ts
│       │   │   ├── WebNotifications.ts
│       │   │   └── Analytics.ts
│       │   ├── utils/          # Web-specific utilities
│       │   │   ├── responsive.ts
│       │   │   ├── keyboard.ts
│       │   │   └── pwa.ts
│       │   └── styles/         # Web-specific styles
│       │       ├── global.css
│       │       └── responsive.css
│       ├── __tests__/          # Web app tests
│       └── cypress/            # Cypress E2E tests
│
├── tools/                      # Development tools and scripts
│   ├── scripts/
│   │   ├── setup.sh           # Development environment setup
│   │   ├── build.sh           # Build script for all platforms
│   │   ├── test.sh            # Test runner for all packages
│   │   └── deploy.sh          # Deployment script
│   ├── configs/
│   │   ├── webpack/           # Webpack configurations
│   │   ├── metro/             # Metro configurations
│   │   └── jest/              # Jest configurations
│   └── generators/            # Code generators (optional)
│       ├── component/
│       ├── screen/
│       └── service/
│
├── docs/                       # Documentation
│   ├── README.md
│   ├── SETUP.md
│   ├── ARCHITECTURE.md
│   ├── API.md
│   ├── DEPLOYMENT.md
│   └── CONTRIBUTING.md
│
└── .github/                   # GitHub workflows
    ├── workflows/
    │   ├── ci.yml             # Continuous Integration
    │   ├── build-android.yml  # Android build workflow
    │   ├── build-ios.yml      # iOS build workflow
    │   └── deploy-web.yml     # Web deployment workflow
    └── ISSUE_TEMPLATE/
```

---

## Code Sharing Strategy

### Shared Code Distribution (Target: 80-90%)

#### Core Business Logic (95% shared)
- **Location**: `packages/shared-core/`
- **Contents**: Types, models, validators, utilities, constants
- **Platform Differences**: None - pure TypeScript logic

#### UI Components (80% shared)
- **Location**: `packages/shared-ui/`
- **Shared Components**: Forms, charts, lists, basic UI elements
- **Platform-Specific**: Navigation patterns, platform-specific UI adaptations

#### Services Layer (85% shared)
- **Location**: `packages/shared-services/`
- **Shared Services**: API clients, storage abstraction, sync logic
- **Platform-Specific**: Notification implementations, file system access

#### Configuration (90% shared)
- **Location**: `packages/shared-config/`
- **Shared Config**: API endpoints, Google Drive settings, app constants
- **Platform-Specific**: Build configurations, deployment settings

### Code Sharing Implementation

```typescript
// Example: Storage abstraction
// packages/shared-services/src/storage/StorageInterface.ts
export interface IStorage {
  get<T>(key: string): Promise<T | null>;
  set<T>(key: string, value: T): Promise<void>;
  remove(key: string): Promise<void>;
  clear(): Promise<void>;
  getAllKeys(): Promise<string[]>;
}

// packages/shared-services/src/storage/WebStorage.ts
export class WebStorage implements IStorage {
  private db: IDBDatabase;
  // IndexedDB implementation
}

// packages/shared-services/src/storage/MobileStorage.ts
export class MobileStorage implements IStorage {
  // AsyncStorage implementation
}
```

---

## Database Abstraction Architecture

### Multi-Layer Storage Strategy

#### Layer 1: Local Storage (Platform-Specific)
```typescript
// Storage abstraction interface
interface ILocalStorage {
  // CRUD operations
  create<T>(collection: string, data: T): Promise<string>;
  read<T>(collection: string, id: string): Promise<T | null>;
  update<T>(collection: string, id: string, data: Partial<T>): Promise<void>;
  delete(collection: string, id: string): Promise<void>;
  
  // Query operations
  findAll<T>(collection: string): Promise<T[]>;
  findWhere<T>(collection: string, predicate: (item: T) => boolean): Promise<T[]>;
  
  // Batch operations
  batchCreate<T>(collection: string, items: T[]): Promise<string[]>;
  batchUpdate<T>(collection: string, updates: Array<{id: string, data: Partial<T>}>): Promise<void>;
}
```

**Mobile Implementation** (AsyncStorage + SQLite):
```typescript
// Enhanced AsyncStorage with SQLite for complex queries
class MobileStorage implements ILocalStorage {
  private sqlite: SQLiteDatabase;
  private asyncStorage: AsyncStorage;
  
  async create<T>(collection: string, data: T): Promise<string> {
    const id = generateId();
    // Store in SQLite for queries
    await this.sqlite.execute(`INSERT INTO ${collection} VALUES (?, ?)`, [id, JSON.stringify(data)]);
    // Cache in AsyncStorage for quick access
    await this.asyncStorage.setItem(`${collection}:${id}`, JSON.stringify(data));
    return id;
  }
}
```

**Web Implementation** (IndexedDB):
```typescript
class WebStorage implements ILocalStorage {
  private db: IDBDatabase;
  
  async create<T>(collection: string, data: T): Promise<string> {
    return new Promise((resolve, reject) => {
      const transaction = this.db.transaction([collection], 'readwrite');
      const store = transaction.objectStore(collection);
      const request = store.add(data);
      request.onsuccess = () => resolve(request.result as string);
      request.onerror = () => reject(request.error);
    });
  }
}
```

#### Layer 2: Cloud Storage (Google Drive)
```typescript
interface ICloudStorage {
  // File operations
  uploadFile(filename: string, content: string): Promise<string>;
  downloadFile(fileId: string): Promise<string>;
  updateFile(fileId: string, content: string): Promise<void>;
  deleteFile(fileId: string): Promise<void>;
  
  // Metadata operations
  listFiles(query?: string): Promise<FileMetadata[]>;
  getFileMetadata(fileId: string): Promise<FileMetadata>;
  
  // Sharing operations
  shareFile(fileId: string, permissions: SharePermissions): Promise<void>;
  getSharedFiles(): Promise<FileMetadata[]>;
}

class GoogleDriveStorage implements ICloudStorage {
  private gapi: GoogleAPI;
  
  async uploadFile(filename: string, content: string): Promise<string> {
    const response = await this.gapi.client.drive.files.create({
      resource: { name: filename, parents: [this.getAppFolderId()] },
      media: { mimeType: 'application/json', body: content }
    });
    return response.result.id!;
  }
}
```

#### Layer 3: Sync Management
```typescript
interface ISyncManager {
  // Sync operations
  syncUp(): Promise<SyncResult>;
  syncDown(): Promise<SyncResult>;
  fullSync(): Promise<SyncResult>;
  
  // Conflict resolution
  resolveConflicts(conflicts: DataConflict[]): Promise<void>;
  
  // Offline queue
  queueOperation(operation: OfflineOperation): Promise<void>;
  processOfflineQueue(): Promise<void>;
}

class SyncManager implements ISyncManager {
  private localStorage: ILocalStorage;
  private cloudStorage: ICloudStorage;
  private conflictResolver: ConflictResolver;
  
  async syncUp(): Promise<SyncResult> {
    const localChanges = await this.getLocalChanges();
    const conflicts: DataConflict[] = [];
    
    for (const change of localChanges) {
      try {
        await this.uploadChange(change);
      } catch (error) {
        if (error.type === 'CONFLICT') {
          conflicts.push(error.conflict);
        }
      }
    }
    
    if (conflicts.length > 0) {
      await this.conflictResolver.resolve(conflicts);
    }
    
    return { success: true, conflicts: conflicts.length };
  }
}
```

### Database Schema Design

#### Core Data Entities
```typescript
// User Profile
interface UserProfile {
  id: string;
  email: string;
  displayName: string;
  preferences: UserPreferences;
  createdAt: Date;
  updatedAt: Date;
}

// Budget
interface Budget {
  id: string;
  name: string;
  description?: string;
  startDate: Date;
  endDate: Date;
  categories: CategoryBudget[];
  collaborators: string[];
  isShared: boolean;
  ownerId: string;
  createdAt: Date;
  updatedAt: Date;
}

// Expense Entry
interface ExpenseEntry {
  id: string;
  budgetId: string;
  amount: number;
  currency: string;
  categoryId: string;
  description?: string;
  date: Date;
  userId: string;
  isRecurring: boolean;
  recurringId?: string;
  attachments?: string[];
  createdAt: Date;
  updatedAt: Date;
}

// Category (Hierarchical)
interface Category {
  id: string;
  name: string;
  parentId?: string;
  level: number;
  budgetId: string;
  color: string;
  icon: string;
  isDefault: boolean;
  customLabel?: string;
  createdAt: Date;
  updatedAt: Date;
}

// Recurring Transaction
interface RecurringTransaction {
  id: string;
  budgetId: string;
  templateExpense: Omit<ExpenseEntry, 'id' | 'date' | 'recurringId'>;
  frequency: RecurrenceFrequency;
  nextExecutionDate: Date;
  isActive: boolean;
  createdAt: Date;
  updatedAt: Date;
}
```

### Offline-First Architecture

#### Offline Queue System
```typescript
interface OfflineOperation {
  id: string;
  type: 'CREATE' | 'UPDATE' | 'DELETE';
  entity: string;
  data: any;
  timestamp: Date;
  retryCount: number;
}

class OfflineQueue {
  private queue: OfflineOperation[] = [];
  
  async enqueue(operation: OfflineOperation): Promise<void> {
    this.queue.push(operation);
    await this.persistQueue();
  }
  
  async processQueue(): Promise<void> {
    const operations = [...this.queue];
    this.queue = [];
    
    for (const operation of operations) {
      try {
        await this.executeOperation(operation);
      } catch (error) {
        if (operation.retryCount < 3) {
          operation.retryCount++;
          this.queue.push(operation);
        } else {
          // Log failed operation
          console.error('Operation failed after 3 retries:', operation);
        }
      }
    }
    
    await this.persistQueue();
  }
}
```

---

## Platform-Specific Considerations

### Mobile App Specific Features

#### Android
```typescript
// android/app/src/main/java/com/consciousbudget/MainActivity.java
public class MainActivity extends ReactActivity {
    @Override
    protected String getMainComponentName() {
        return "ConsciousBudget";
    }
    
    // Biometric authentication integration
    // File system access for exports
    // Background sync capabilities
}
```

#### iOS
```swift
// ios/ConsciousBudget/AppDelegate.m
@implementation AppDelegate
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Biometric authentication setup
    // Background app refresh configuration
    // Keychain access for secure storage
    return YES;
}
@end
```

#### Mobile-Specific Services
```typescript
// Mobile notification service
class MobileNotificationService {
  async scheduleRecurringReminder(frequency: 'daily' | 'weekly'): Promise<void> {
    const { PushNotificationIOS } = require('@react-native-community/push-notification-ios');
    const { Notifications } = require('expo-notifications');
    
    if (Platform.OS === 'ios') {
      // iOS implementation
    } else {
      // Android implementation
    }
  }
  
  async requestPermissions(): Promise<boolean> {
    // Platform-specific permission requests
  }
}
```

### Web App Specific Features

#### Progressive Web App (PWA)
```typescript
// Web service worker for offline functionality
// public/sw.js
self.addEventListener('fetch', (event) => {
  if (event.request.url.includes('/api/')) {
    event.respondWith(
      caches.match(event.request)
        .then(response => response || fetch(event.request))
        .catch(() => caches.match('/offline.html'))
    );
  }
});

// Web notification service
class WebNotificationService {
  async requestPermission(): Promise<boolean> {
    if ('Notification' in window) {
      const permission = await Notification.requestPermission();
      return permission === 'granted';
    }
    return false;
  }
  
  async scheduleNotification(title: string, options: NotificationOptions): Promise<void> {
    if (this.hasPermission()) {
      new Notification(title, options);
    }
  }
}
```

#### Web-Specific Responsive Design
```typescript
// Responsive layout hooks
const useResponsiveLayout = () => {
  const [isMobile, setIsMobile] = useState(false);
  const [isTablet, setIsTablet] = useState(false);
  
  useEffect(() => {
    const handleResize = () => {
      setIsMobile(window.innerWidth < 768);
      setIsTablet(window.innerWidth >= 768 && window.innerWidth < 1024);
    };
    
    handleResize();
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  
  return { isMobile, isTablet, isDesktop: !isMobile && !isTablet };
};
```

---

## Development Workflow

### Development Environment Setup

#### Root Package.json Configuration
```json
{
  "name": "conscious-budget-monorepo",
  "private": true,
  "workspaces": {
    "packages": [
      "packages/*",
      "apps/*"
    ]
  },
  "scripts": {
    "bootstrap": "yarn install && yarn setup:all",
    "setup:all": "yarn workspace shared-core build && yarn workspace shared-ui build && yarn workspace shared-services build",
    "dev:mobile": "yarn workspace mobile start",
    "dev:web": "yarn workspace web start",
    "build:all": "yarn workspaces run build",
    "test:all": "yarn workspaces run test",
    "lint:all": "yarn workspaces run lint",
    "type-check:all": "yarn workspaces run type-check"
  },
  "devDependencies": {
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "@typescript-eslint/parser": "^6.0.0",
    "eslint": "^8.45.0",
    "prettier": "^3.0.0",
    "typescript": "^5.1.6",
    "jest": "^29.6.1",
    "lerna": "^7.1.4"
  }
}
```

#### TypeScript Configuration (Root)
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "baseUrl": ".",
    "paths": {
      "@conscious-budget/shared-core": ["packages/shared-core/src"],
      "@conscious-budget/shared-ui": ["packages/shared-ui/src"],
      "@conscious-budget/shared-services": ["packages/shared-services/src"],
      "@conscious-budget/shared-config": ["packages/shared-config/src"]
    }
  },
  "include": [
    "packages/*/src/**/*",
    "apps/*/src/**/*"
  ],
  "exclude": [
    "**/node_modules",
    "**/dist",
    "**/build"
  ]
}
```

### Build and Deployment Strategy

#### Continuous Integration Pipeline
```yaml
# .github/workflows/ci.yml
name: CI Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'yarn'
      
      - name: Install dependencies
        run: yarn bootstrap
      
      - name: Type checking
        run: yarn type-check:all
      
      - name: Lint
        run: yarn lint:all
      
      - name: Test
        run: yarn test:all
  
  build-mobile:
    needs: test
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup React Native environment
        # Setup Android SDK, iOS certificates, etc.
      
      - name: Build Android
        run: |
          cd apps/mobile
          yarn android:build
      
      - name: Build iOS
        run: |
          cd apps/mobile
          yarn ios:build
  
  build-web:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build web app
        run: |
          cd apps/web
          yarn build
      
      - name: Deploy to hosting
        # Deploy to Firebase Hosting, Netlify, etc.
```

---

## Implementation Phases

### Phase 1: Foundation (Weeks 1-3)
**Objective**: Set up monorepo structure and core abstractions

#### Week 1: Project Setup
- [ ] Initialize monorepo with yarn workspaces
- [ ] Set up shared packages structure
- [ ] Configure TypeScript, ESLint, Prettier
- [ ] Set up basic CI/CD pipeline
- [ ] Create basic documentation

#### Week 2: Core Business Logic
- [ ] Implement data models (Expense, Budget, Category, User)
- [ ] Create TypeScript type definitions
- [ ] Build utility functions (date, currency, validation)
- [ ] Set up constants and configuration
- [ ] Write unit tests for core logic

#### Week 3: Storage Abstraction
- [ ] Design and implement storage interfaces
- [ ] Create platform-specific storage implementations
- [ ] Build basic CRUD operations
- [ ] Implement data validation layer
- [ ] Set up local storage for both platforms

### Phase 2: UI Foundation (Weeks 4-6)
**Objective**: Build shared UI components and basic navigation

#### Week 4: Component Library
- [ ] Create design system (colors, typography, spacing)
- [ ] Build basic UI components (Button, Input, Card)
- [ ] Implement form components (ExpenseForm, CategorySelector)
- [ ] Set up component documentation
- [ ] Create responsive layout components

#### Week 5: State Management
- [ ] Set up Redux store with Redux Toolkit
- [ ] Create slices for expenses, budgets, categories
- [ ] Implement custom hooks for data management
- [ ] Build offline queue management
- [ ] Add state persistence

#### Week 6: Navigation & Screens
- [ ] Implement mobile navigation (React Navigation)
- [ ] Create web routing (React Router)
- [ ] Build basic screen components
- [ ] Set up deep linking
- [ ] Create responsive layouts

### Phase 3: Core Features (Weeks 7-10)
**Objective**: Implement main application features

#### Week 7: Expense Management
- [ ] Quick expense entry interface
- [ ] Expense listing and filtering
- [ ] Category selection and management
- [ ] Basic expense editing and deletion
- [ ] Form validation and error handling

#### Week 8: Budget Management
- [ ] Budget creation and editing
- [ ] Budget allocation across categories
- [ ] Real-time budget tracking
- [ ] Budget progress visualization
- [ ] Multiple budget support

#### Week 9: Categories & Hierarchies
- [ ] Hierarchical category system
- [ ] Category duplication with labels
- [ ] Custom category creation
- [ ] Category management interface
- [ ] Category-based filtering

#### Week 10: Reporting & Analytics
- [ ] Basic spending reports
- [ ] Chart components (pie, bar, line)
- [ ] Date range filtering
- [ ] Export functionality
- [ ] Comparative analysis

### Phase 4: Cloud Integration (Weeks 11-13)
**Objective**: Implement Google Drive integration and sync

#### Week 11: Google Authentication
- [ ] Google OAuth 2.0 integration
- [ ] Secure token storage
- [ ] Authentication state management
- [ ] Logout and session handling
- [ ] Account linking flow

#### Week 12: Google Drive Storage
- [ ] Google Drive API integration
- [ ] File upload and download
- [ ] Folder structure management
- [ ] File sharing mechanisms
- [ ] Error handling and retry logic

#### Week 13: Synchronization
- [ ] Sync manager implementation
- [ ] Conflict resolution algorithms
- [ ] Offline queue processing
- [ ] Background sync capabilities
- [ ] Sync status indicators

### Phase 5: Advanced Features (Weeks 14-16)
**Objective**: Add recurring transactions, notifications, and collaboration

#### Week 14: Recurring Transactions
- [ ] Recurring transaction templates
- [ ] Frequency configuration
- [ ] Automated execution
- [ ] Modification and cancellation
- [ ] Notification integration

#### Week 15: Notifications & Reminders
- [ ] Local notification system
- [ ] Push notification setup
- [ ] Reminder scheduling
- [ ] User preference management
- [ ] Smart notification timing

#### Week 16: Collaboration Features
- [ ] Budget sharing mechanisms
- [ ] Multi-user expense entry
- [ ] User attribution system
- [ ] Real-time synchronization
- [ ] Permission management

### Phase 6: Polish & Performance (Weeks 17-18)
**Objective**: Optimize performance and prepare for release

#### Week 17: Performance Optimization
- [ ] Bundle size optimization
- [ ] Lazy loading implementation
- [ ] Memory usage optimization
- [ ] Database query optimization
- [ ] Image and asset optimization

#### Week 18: Final Polish
- [ ] Comprehensive testing (unit, integration, E2E)
- [ ] Accessibility improvements
- [ ] Error boundary implementation
- [ ] Performance monitoring setup
- [ ] Documentation completion

### Phase 7: Deployment (Weeks 19-20)
**Objective**: Deploy applications to app stores and web

#### Week 19: Build & Release Preparation
- [ ] Production build optimization
- [ ] App store asset preparation
- [ ] Release notes and documentation
- [ ] Beta testing setup
- [ ] Analytics integration

#### Week 20: Deployment & Launch
- [ ] Web application deployment
- [ ] Android Play Store submission
- [ ] iOS App Store submission
- [ ] Monitoring and error tracking setup
- [ ] Post-launch support preparation

---

## Conclusion

This project structure provides a solid foundation for building the ConsciousBudget application with maximum code sharing between platforms while maintaining the flexibility to implement platform-specific features. The modular architecture ensures maintainability and scalability as the application grows.

### Key Benefits of This Structure

1. **High Code Reuse**: 80-90% code sharing as specified in requirements
2. **Maintainability**: Clear separation of concerns and modular design
3. **Scalability**: Architecture supports growth and additional features
4. **Performance**: Optimized for offline-first functionality
5. **Developer Experience**: Well-organized structure with clear conventions
6. **Testing**: Comprehensive testing strategy at all levels
7. **CI/CD Ready**: Automated build and deployment pipelines

### Next Steps

1. Review and approve this project structure
2. Set up the initial monorepo structure
3. Begin with Phase 1 implementation
4. Establish development workflows and conventions
5. Set up project management and tracking tools

This structure will serve as the foundation for implementing the complete ConsciousBudget application according to the specified requirements.