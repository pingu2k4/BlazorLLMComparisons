# BigPickle Knowledge Base - Implementation Plan

## Product Name & Pitch
**NexusHub** - A modern team knowledge base where teams organize, collaborate, and share knowledge through beautiful markdown articles with powerful workflow controls.

## Theme/Branding Plan

### Visual Identity Keywords
- Clean, professional, editorial
- Modern yet approachable
- Focus-driven with subtle accent colors
- High readability for long-form content

### Color Palette
- **Primary**: #4F46E5 (Indigo) - strong but professional
- **Secondary**: #06B6D4 (Cyan) - energetic accent
- **Neutral**: #64748B (Slate) - professional gray tones
- **Background**: #FFFFFF / #F8FAFC (clean whites)
- **Text**: #1E293B / #475569 (contrast hierarchy)
- **Semantic**: 
  - Success: #10B981 (Emerald)
  - Warning: #F59E0B (Amber) 
  - Error: #EF4444 (Red)
  - Info: #3B82F6 (Blue)

### Typography
- **System fonts**: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif
- **Headings**: Weight 600-700, tighter letter-spacing
- **Body**: Weight 400, comfortable line height (1.6) for reading
- **UI Elements**: Weight 500, consistent sizing

### Spacing/Radius/Shadows
- **Spacing**: 4px base unit (8px = 2rem, 16px = 4rem)
- **Border Radius**: 6px standard, 8px for cards, 12px for modals
- **Shadows**: Subtle (0 1px 3px rgba(0,0,0,0.1)), medium (0 4px 6px rgba(0,0,0,0.1))

### Component Styling Rules
- **Buttons**: Primary (indigo fill), Secondary (outlined), Ghost (transparent)
- **Inputs**: Clean borders, focus rings with indigo, consistent heights
- **Cards**: Subtle shadows, hover states, clear information hierarchy
- **Tables**: Alternating rows, clean borders, sortable headers
- **Alerts**: Color-coded left borders, semantic icons

### Brand Differentiation
- Custom CSS variables override component library defaults
- Unique micro-interactions (hover states, transitions)
- Consistent spacing system across all components
- Professional color palette avoiding primary colors in component libraries

## Roles/Permissions Matrix

| Action | Owner | Member |
|--------|-------|--------|
| Create workspace | ✅ | ❌ |
| Delete workspace | ✅ | ❌ |
| Manage members | ✅ | ❌ |
| Invite users | ✅ | ❌ |
| Create article | ✅ | ✅ |
| Edit own article | ✅ | ✅ |
| Edit others' articles | ✅ | ✅ |
| Submit for review | ✅ | ✅ |
| Approve/publish | ✅ | ❌ |
| Unpublish/archive | ✅ | ❌ |
| Manage tags | ✅ | ❌ |
| Delete any comment | ✅ | ❌ |
| Delete own comment | ✅ | ✅ |
| Bookmark articles | ✅ | ✅ |

## Page Map & User Flows

### Core Routes
- `/` - Landing/workspace overview
- `/auth/login` - Login page
- `/auth/register` - Registration page
- `/workspace/new` - Create workspace
- `/workspace/{id}` - Workspace dashboard
- `/workspace/{id}/articles` - Article list with filters
- `/workspace/{id}/articles/new` - Create article
- `/workspace/{id}/articles/{id}` - Article view
- `/workspace/{id}/articles/{id}/edit` - Edit article
- `/workspace/{id}/articles/{id}/history` - Revision history
- `/workspace/{id}/tags` - Tag management
- `/workspace/{id}/members` - Member management
- `/workspace/{id}/bookmarks` - User bookmarks
- `/invite/{token}` - Accept invitation

### Key User Flows

#### Article Creation Flow
1. User selects workspace → clicks "New Article"
2. Form: title, summary, tags, markdown editor with preview
3. Save as Draft → submit for review → Owner approves → Published

#### Invitation Flow
1. Owner generates invite link for workspace
2. Recipient clicks link → accepts invitation
3. Added as Member with proper access

## Architecture

### Projects Structure
```
src/BigPickle/
├── BigPickle.AppHost/          # Aspire orchestration
├── BigPickle.Api/              # Minimal APIs (backend)
├── BigPickle.Core/             # Domain models, interfaces
├── BigPickle.Infrastructure/  # Data access, external services
├── BigPickle.Web/              # Blazor WebAssembly frontend
└── BigPuckle.Shared/           # Shared DTOs, contracts
```

### Boundaries & Rationale
- **AppHost**: Aspire orchestration for distributed deployment
- **Api**: Thin HTTP layer using Minimal APIs, validates requests, calls services
- **Core**: Domain logic, entities, interfaces (no dependencies)
- **Infrastructure**: EF Core, password hashing, email services
- **Web**: Blazor WASM with Auto mode, static SSR where possible
- **Shared**: Data contracts between API and Web

### Communication Flow
Web → Http API → Services → Infrastructure → Database

## Data Strategy

### Persistence Choice
**PostgreSQL with Entity Framework Core**
- Mature, reliable with .NET
- Strong typing and migrations
- Good for complex queries needed for article search

### Key Data Shapes

#### Workspace
```csharp
public class Workspace
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime UpdatedAt { get; set; }
    public ICollection<WorkspaceMember> Members { get; set; }
}
```

#### Article
```csharp
public class Article
{
    public Guid Id { get; set; }
    public Guid WorkspaceId { get; set; }
    public Guid AuthorId { get; set; }
    public string Title { get; set; }
    public string Summary { get; set; }
    public string MarkdownContent { get; set; }
    public ArticleStatus Status { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime UpdatedAt { get; set; }
    public ICollection<ArticleTag> Tags { get; set; }
    public ICollection<ArticleRevision> Revisions { get; set; }
}
```

#### Indexing Strategy
- Composite indexes on WorkspaceId + Status for article listing
- Full-text search index on Title, Summary, MarkdownContent
- Indexes on Tag usage counts
- Indexes on UserId for bookmark queries

### Schema Evolution
- EF Core migrations with automatic versioning
- Backwards-compatible migrations for production
- Database snapshots for major version changes

## API Surface Summary

### Authentication Endpoints
- `POST /api/auth/register`
- `POST /api/auth/login`
- `POST /api/auth/logout`
- `GET /api/auth/me`

### Workspace Endpoints
- `GET /api/workspaces` (user's workspaces)
- `POST /api/workspaces`
- `GET /api/workspaces/{id}`
- `PUT /api/workspaces/{id}`
- `GET /api/workspaces/{id}/members`
- `POST /api/workspaces/{id}/members`
- `DELETE /api/workspaces/{id}/members/{userId}`
- `POST /api/workspaces/{id}/invite`

### Article Endpoints
- `GET /api/workspaces/{id}/articles` (with pagination/filtering)
- `POST /api/workspaces/{id}/articles`
- `GET /api/workspaces/{id}/articles/{id}`
- `PUT /api/workspaces/{id}/articles/{id}`
- `POST /api/workspaces/{id}/articles/{id}/submit-review`
- `POST /api/workspaces/{id}/articles/{id}/publish`
- `POST /api/workspaces/{id}/articles/{id}/unpublish`
- `GET /api/workspaces/{id}/articles/{id}/revisions`
- `POST /api/workspaces/{id}/articles/{id}/bookmark`

### Comments Endpoints
- `GET /api/articles/{id}/comments`
- `POST /api/articles/{id}/comments`
- `DELETE /api/comments/{id}`

### Key DTOs
- `PaginatedResponse<T>` with `PageIndex`, `PageSize`, `TotalCount`, `Items`
- `ArticleFilter` with `Search`, `Tags`, `Status`, `SortBy`
- `CreateArticleRequest`, `UpdateArticleRequest`
- `InviteRequest`, `AcceptInviteRequest`

## UI Plan - Reusable Components

### 1. ConfirmDialog Component
**Usage**: Delete operations, publish confirmations, member removal
- Reusable modal with customizable title/message
- Async confirm/cancel callbacks
**Locations**: Article deletion, member management, tag deletion, unpublish actions

### 2. TagPicker Component  
**Usage**: Article creation/editing, article filtering
- Multi-select with autocomplete
- Tag creation inline (owners only)
- Visual tag pills with remove buttons
**Locations**: Article form, article list filters

### 3. MarkdownEditor Component
**Usage**: Article creation and editing
- Split-pane or tabbed preview
- Syntax highlighting in editor
- Toolbar for common markdown operations
**Locations**: Create article, edit article pages

### 4. NotificationToast Component
**Usage**: Global user feedback
- Auto-dismiss after configurable time
- Multiple toast types (success, error, warning, info)
- Queue management for multiple notifications
**Locations**: Used throughout application after CRUD operations

### Additional Reusable Components
- `DataGrid<T>` - Used for article lists, member lists, tag lists
- `ValidatedInput<T>` - Form inputs with validation display
- `StatusBadge` - Article status, role display
- `LoadingSpinner` - Consistent loading states

## Aspire Integration Plan

### Projects to Register
```csharp
// BigPickle.AppHost/Program.cs
builder.AddProject<Projects.BigPickle_Api>("api");
builder.AddProject<Projects.BigPickle_Web>("web")
    .WithExternalHttpEndpoints()
    .WithExplicitStart();
```

### Resources to Add
```csharp
var postgres = builder.AddPostgres("postgres")
    .WithEnvironment("POSTGRES_DB", "nexusdb");

builder.AddPostgresDatabase<BigPickleDbContext>(postgres, "nexusdb");
```

### Configuration Management
- Connection strings via Aspire resource references
- Development: local PostgreSQL container
- Production: Azure PostgreSQL or similar managed service
- Health checks for database connectivity

## Step-by-Step Execution Checklist

### Phase 1: Foundation Setup
- [ ] Create BigPickle solution structure under `src/BigPickle/`
- [ ] Set up all 6 projects with proper dependencies
- [ ] Configure EF Core with PostgreSQL
- [ ] Set up Aspire AppHost integration
- [ ] Create base authentication infrastructure

### Phase 2: Core Domain & Data Layer
- [ ] Implement domain entities (Workspace, User, Article, etc.)
- [ ] Create EF Core DbContext and initial migration
- [ ] Implement password hashing and JWT token generation
- [ ] Set up authorization policies for Owner/Member roles
- [ ] Create repository/service layer

### Phase 3: Authentication & Workspace Management
- [ ] Implement registration/login APIs
- [ ] Create workspace CRUD APIs
- [ ] Implement member management APIs
- [ ] Build invitation workflow APIs
- [ ] Create corresponding Blazor pages

### Phase 4: Article System
- [ ] Implement article CRUD APIs with workflow states
- [ ] Build markdown rendering with sanitization
- [ ] Create article revision system
- [ ] Implement search and filtering
- [ ] Build article UI components (editor, viewer, list)

### Phase 5: Advanced Features
- [ ] Implement tag management system
- [ ] Build comment system with moderation
- [ ] Add bookmark functionality
- [ ] Create reusable UI components
- [ ] Implement responsive design

### Phase 6: Polish & Production Readiness
- [ ] Add comprehensive error handling
- [ ] Implement structured logging and health checks
- [ ] Add loading, empty, and error states
- [ ] Optimize performance and database queries
- [ ] Final testing and zero-warning build verification

### Phase 7: Integration & Deployment
- [ ] Configure Aspire for production deployment
- [ ] Set up database migrations
- [ ] Test complete application flows
- [ ] Verify all constraints and requirements met
- [ ] Final code review and documentation