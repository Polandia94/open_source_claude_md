# Django Framework - AI Assistant Context Guide

## Project Overview

**Django** is a high-level Python web framework that encourages rapid development and clean, pragmatic design. It follows the Model-Template-View (MTV) architectural pattern and includes an extensive set of built-in features for common web development tasks.

- **Repository**: https://github.com/django/django
- **Primary Language**: Python
- **License**: BSD-3-Clause
- **Python Support**: >= 3.12 (currently supporting 3.12, 3.13, 3.14)
- **Current Version**: 6.1.0-alpha
- **Issue Tracking**: Django uses Trac (https://code.djangoproject.com/) for bug tracking and feature requests

## Core Philosophy

Django's design philosophy centers around:

1. **Don't Repeat Yourself (DRY)**: Reduce redundancy and promote reusability
2. **Loose Coupling**: Components should be independent and interchangeable
3. **Explicit is Better Than Implicit**: Clear, readable code over clever tricks
4. **Rapid Development**: Provide tools to build quickly without sacrificing quality
5. **Batteries Included**: Comprehensive built-in functionality for common tasks

## Project Structure

```
django/
├── apps/              # Application configuration and registry
├── conf/              # Settings, configuration, and locale formats
├── contrib/           # Optional built-in applications
│   ├── admin/         # Admin interface
│   ├── auth/          # Authentication framework
│   ├── contenttypes/  # Content type framework
│   ├── sessions/      # Session framework
│   ├── messages/      # Messaging framework
│   ├── staticfiles/   # Static file handling
│   ├── gis/           # Geographic framework
│   ├── postgres/      # PostgreSQL-specific features
│   ├── sites/         # Multi-site support
│   ├── sitemaps/      # Sitemap generation
│   ├── syndication/   # RSS/Atom feed framework
│   └── ...
├── core/              # Core utilities and infrastructure
│   ├── cache/         # Caching framework
│   ├── checks/        # System checks framework
│   ├── files/         # File handling
│   ├── handlers/      # WSGI/ASGI handlers
│   ├── mail/          # Email backend
│   ├── management/    # Management commands (django-admin)
│   ├── serializers/   # Serialization framework
│   └── ...
├── db/                # Database abstraction layer
│   ├── backends/      # Database backends (PostgreSQL, MySQL, SQLite, Oracle)
│   ├── migrations/    # Schema migration framework
│   └── models/        # ORM implementation
├── forms/             # Forms framework
├── http/              # HTTP request/response handling
├── middleware/        # Middleware components
├── template/          # Template engine
├── templatetags/      # Built-in template tags
├── test/              # Testing framework
├── urls/              # URL routing
├── utils/             # Utility functions and helpers
├── views/             # Generic views
└── shortcuts.py       # Common shortcut functions

tests/                 # Comprehensive test suite (200+ test modules)
docs/                  # Documentation source files
scripts/               # Release and maintenance scripts
```

## Key Architectural Patterns

### 1. Model-Template-View (MTV) Architecture

Django uses MTV, a variation of MVC:

- **Model**: Data layer (ORM models in `django.db.models`)
- **Template**: Presentation layer (template system in `django.template`)
- **View**: Business logic layer (view functions/classes in `django.views`)
- **URL Dispatcher**: Routes requests to appropriate views

### 2. Metaclass-Based Model System

Django's ORM uses metaclasses extensively:

```python
class ModelBase(type):
    """Metaclass for all models."""
    # Handles model class creation, field registration, and Meta options
```

Key features:
- `ModelBase` metaclass processes model definitions
- Automatic field registration via `contribute_to_class()`
- Meta class for model configuration
- Dynamic exception creation (DoesNotExist, MultipleObjectsReturned)

### 3. Application Registry

The `django.apps` module manages installed applications:

```python
class AppConfig:
    """Class representing a Django application and its configuration."""
    # Stores app metadata, models, and configuration
```

- Centralized registry of all installed apps
- Lazy model loading and relationship resolution
- App-specific configuration via AppConfig subclasses

### 4. Database Backend Abstraction

Support for multiple databases through a unified interface:

- Base backend in `django.db.backends.base`
- Database-specific implementations: PostgreSQL, MySQL, SQLite, Oracle
- Database routers for multi-database support
- Schema editor for migrations

### 5. QuerySet API

Lazy, chainable, and database-agnostic query interface:

```python
# QuerySets are lazy - database queries only execute when needed
articles = Article.objects.filter(published=True).order_by('-pub_date')[:5]
```

Features:
- Lazy evaluation
- Method chaining
- Query optimization (select_related, prefetch_related)
- Database-agnostic lookups and expressions

### 6. Middleware Stack

Request/response processing pipeline:

```python
# Middleware wraps view processing
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # ... more middleware
]
```

### 7. Signal Dispatcher

Decoupled event handling system:

```python
from django.db.models.signals import post_save

@receiver(post_save, sender=MyModel)
def my_handler(sender, instance, **kwargs):
    # Handle post-save event
```

### 8. Class-Based Views

Reusable view components with mixins:

```python
class View:
    """Intentionally simple parent class for all views."""

    @classonlymethod
    def as_view(cls, **initkwargs):
        """Main entry point for a request-response process."""
```

Features:
- Method dispatch based on HTTP method
- Mixin composition for reusability
- Generic views for common patterns (ListView, DetailView, etc.)

### 9. Template Engine

Flexible template system with:
- Template inheritance and blocks
- Context processors for global context
- Custom template tags and filters
- Multiple backend support (Django templates, Jinja2)

### 10. Forms Framework

Declarative form definition with validation:

```python
class DeclarativeFieldsMetaclass(MediaDefiningClass):
    """Collect Fields declared on the base classes."""
```

Features:
- Automatic HTML generation
- Data cleaning and validation
- ModelForm for model-backed forms
- Formsets for multiple forms

## Technology Stack

### Core Dependencies

```toml
dependencies = [
    "asgiref>=3.9.1",      # ASGI support for async views
    "sqlparse>=0.5.0",     # SQL parsing and formatting
    "tzdata; sys_platform == 'win32'",  # Timezone data for Windows
]
```

### Optional Dependencies

```toml
[project.optional-dependencies]
argon2 = ["argon2-cffi>=23.1.0"]  # Argon2 password hashing
bcrypt = ["bcrypt>=4.1.1"]        # bcrypt password hashing
```

### Development Dependencies

Key testing and development tools:
- **black**: Code formatting (target Python 3.12+)
- **isort**: Import sorting
- **flake8**: Linting
- **tox**: Testing across environments
- **Selenium**: Browser testing
- **pre-commit**: Git hooks for code quality

Database-specific requirements:
- PostgreSQL: psycopg2-binary, geoip2
- MySQL: mysqlclient
- Oracle: cx_Oracle

## Coding Standards and Conventions

### Python Style

1. **Code Formatting**:
   - Use Black auto-formatter (88 character line length)
   - 4 spaces for indentation (Python), 2 spaces (HTML)
   - Follow PEP 8 with Django-specific exceptions
   - Docstrings and comments wrapped at 79 characters

2. **Naming Conventions**:
   - `snake_case` for variables, functions, methods
   - `InitialCaps` for classes
   - `UPPER_CASE` for constants
   - Use descriptive names, avoid abbreviations

3. **String Formatting**:
   - f-strings for simple variable interpolation
   - `str.format()` or `%` for complex formatting
   - Never use f-strings for translatable strings

4. **Import Organization** (enforced by isort):
   ```python
   # Future imports
   from __future__ import annotations

   # Standard library
   import os
   from pathlib import Path

   # Third-party libraries
   import asgiref

   # Django imports
   from django.db import models

   # Local imports
   from .models import MyModel
   ```

5. **Comments and Docstrings**:
   - Follow PEP 257 for docstrings
   - Avoid "we" in comments (use "Loop over" not "We loop over")
   - Document WHY, not WHAT (code should be self-explanatory)

### Django-Specific Patterns

1. **Model Definitions**:
   ```python
   class MyModel(models.Model):
       name = models.CharField(max_length=100)
       created = models.DateTimeField(auto_now_add=True)

       class Meta:
           ordering = ['-created']
           verbose_name = 'my model'
           verbose_name_plural = 'my models'

       def __str__(self):
           return self.name

       def get_absolute_url(self):
           return reverse('mymodel_detail', kwargs={'pk': self.pk})
   ```

2. **QuerySet Methods**:
   - Return QuerySets for chaining
   - Use meaningful names
   - Document expected behavior

3. **View Classes**:
   - Inherit from appropriate base classes
   - Override specific methods, not entire flows
   - Use mixins for cross-cutting concerns

4. **URL Patterns**:
   ```python
   from django.urls import path, include

   urlpatterns = [
       path('articles/', views.article_list, name='article_list'),
       path('articles/<int:pk>/', views.article_detail, name='article_detail'),
   ]
   ```

## Testing Strategy

### Test Organization

Django has a comprehensive test suite with 200+ test modules:

```
tests/
├── admin_*/          # Admin interface tests
├── auth_tests/       # Authentication tests
├── db_*/             # Database and ORM tests
├── forms_*/          # Forms framework tests
├── generic_views/    # Generic views tests
├── migrations/       # Migration framework tests
├── template_*/       # Template engine tests
└── ...
```

### Test Running

```bash
# Run all tests
cd tests
python runtests.py

# Run specific tests
python runtests.py admin_views auth_tests

# Run with specific database backend
python runtests.py --settings=test_sqlite
```

### Test Classes

Django provides several test case classes:

```python
from django.test import TestCase, TransactionTestCase, SimpleTestCase

class MyModelTest(TestCase):
    """Uses database transactions for isolation."""

    def setUp(self):
        # Set up test data
        pass

    def test_model_creation(self):
        """Expected behavior description."""
        obj = MyModel.objects.create(name='test')
        self.assertEqual(obj.name, 'test')
```

### Testing Best Practices

1. **Test Naming**:
   - Descriptive method names: `test_user_can_login_with_email()`
   - Docstrings state expected behavior
   - Don't include "Tests that" or "Ensures that" preambles

2. **Assertions**:
   - Use `assertRaisesMessage()` instead of `assertRaises()`
   - Use `assertIs(x, True/False)` for boolean values
   - Test both success and failure cases

3. **Test Isolation**:
   - Each test should be independent
   - Use `setUp()` for common test data
   - TestCase provides automatic transaction rollback

4. **Database Tests**:
   - Use `TestCase` for database-dependent tests
   - Use `TransactionTestCase` when testing transactions
   - Use `SimpleTestCase` for non-database tests (faster)

## Common Development Tasks

### Working with Models

```python
# Define a model
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    pub_date = models.DateTimeField(auto_now_add=True)
    author = models.ForeignKey('auth.User', on_delete=models.CASCADE)

    class Meta:
        ordering = ['-pub_date']
        indexes = [
            models.Index(fields=['-pub_date']),
        ]

    def __str__(self):
        return self.title

# Query the model
recent_articles = Article.objects.filter(
    pub_date__gte=timezone.now() - timedelta(days=7)
).select_related('author')
```

### Creating Migrations

```python
# After model changes
python manage.py makemigrations

# Apply migrations
python manage.py migrate

# Custom migration
from django.db import migrations

class Migration(migrations.Migration):
    dependencies = [
        ('myapp', '0001_initial'),
    ]

    operations = [
        migrations.AddField(
            model_name='article',
            name='slug',
            field=models.SlugField(),
        ),
    ]
```

### Writing Views

```python
# Function-based view
from django.shortcuts import render, get_object_or_404
from django.http import HttpResponse

def article_detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    return render(request, 'articles/detail.html', {'article': article})

# Class-based view
from django.views.generic import ListView

class ArticleListView(ListView):
    model = Article
    template_name = 'articles/list.html'
    context_object_name = 'articles'
    paginate_by = 10

    def get_queryset(self):
        return Article.objects.filter(published=True)
```

### Management Commands

```python
# django/core/management/commands/mycommand.py
from django.core.management.base import BaseCommand

class Command(BaseCommand):
    help = 'Description of what this command does'

    def add_arguments(self, parser):
        parser.add_argument('--option', type=str, help='Optional argument')

    def handle(self, *args, **options):
        self.stdout.write(self.style.SUCCESS('Command executed successfully'))
```

## Contribution Workflow

### Important: Trac Ticket Requirement

**CRITICAL**: Django uses Trac (not GitHub Issues) for tracking bugs and features.

- All non-trivial pull requests require a Trac ticket: https://code.djangoproject.com/newticket
- Pull requests without tickets will be closed
- Search existing tickets before creating new ones
- Reference ticket numbers in commits: `Fixed #12345 -- Description`

### Understanding PR Context

**CRITICAL for AI Assistants**: Before making any changes, thoroughly understand the PR/ticket context:

1. **Read the ticket/PR description**: The description often references specific code changes made in previous tickets
2. **Check linked issues**: Many PRs reference previous tickets (e.g., "Refs #27100") that provide context
3. **Understand the "why"**: Documentation PRs often remove obsolete messages left from previous implementations
4. **Stay focused**: If a PR says "remove obsolete message from tutorial and migrations topic", only touch those specific files
5. **Follow the trail**: When a PR references a past ticket number, that ticket explains what changed and why documentation is now obsolete

### Git Workflow

1. **Fork and Clone**:
   ```bash
   git clone https://github.com/django/django.git
   cd django
   git remote add upstream https://github.com/django/django.git
   ```

2. **Create Feature Branch**:
   ```bash
   git checkout -b ticket_12345
   ```

3. **Make Changes**:
   - Follow coding standards
   - Add tests for new functionality
   - Update documentation if needed

4. **Run Tests**:
   ```bash
   cd tests
   python runtests.py
   ```

5. **Run Code Quality Checks**:
   ```bash
   # Install pre-commit
   pip install pre-commit
   pre-commit install

   # Or run manually
   black --check .
   isort --check-only django tests
   flake8 .
   ```

6. **Commit Changes**:
   ```bash
   git add .
   git commit -m "Fixed #12345 -- Description of the change"
   ```

   Commit message format:
   - First line: `Fixed #ticket -- Short description`
   - Use present tense: "Add feature" not "Added feature"
   - Reference ticket numbers

7. **Push and Create Pull Request**:
   ```bash
   git push origin ticket_12345
   ```

### Code Review Process

1. Pull requests are reviewed by Django core developers
2. Address review feedback promptly
3. Keep commits focused and atomic
4. Squash commits if requested
5. Be patient - reviews may take time

## Database Backends

Django supports multiple database backends with a unified API:

### PostgreSQL (Recommended)

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydatabase',
        'USER': 'mydatabaseuser',
        'PASSWORD': 'mypassword',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
```

Features:
- Full-text search
- Array fields, JSON fields
- Database-level constraints
- Advanced indexing (GiST, GIN)

### MySQL

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'OPTIONS': {
            'charset': 'utf8mb4',
        },
    }
}
```

### SQLite

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

Default for development; not recommended for production.

### Oracle

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.oracle',
    }
}
```

## Security Features

Django includes extensive security features:

1. **CSRF Protection**: Automatic CSRF token validation
2. **XSS Protection**: Template auto-escaping
3. **SQL Injection Prevention**: Parameterized queries
4. **Clickjacking Protection**: X-Frame-Options middleware
5. **SSL/HTTPS**: Redirect to HTTPS, secure cookies
6. **Password Hashing**: PBKDF2, Argon2, bcrypt support
7. **Security Middleware**: Multiple security headers

## Performance Optimization

### Query Optimization

```python
# Bad: N+1 queries
for article in Article.objects.all():
    print(article.author.name)  # Separate query for each author

# Good: Use select_related for foreign keys
for article in Article.objects.select_related('author'):
    print(article.author.name)  # Single JOIN query

# Good: Use prefetch_related for reverse relations
authors = Author.objects.prefetch_related('articles')
```

### Caching

```python
from django.core.cache import cache

# Cache a value
cache.set('my_key', 'my_value', timeout=300)

# Get cached value
value = cache.get('my_key')

# Cache decorator
from django.views.decorators.cache import cache_page

@cache_page(60 * 15)
def my_view(request):
    ...
```

### Database Indexing

```python
class MyModel(models.Model):
    field1 = models.CharField(max_length=100, db_index=True)

    class Meta:
        indexes = [
            models.Index(fields=['field1', 'field2']),
            models.Index(fields=['-created_at']),
        ]
```

## Async Support

Django 3.1+ supports async views and middleware:

```python
from django.http import HttpResponse
import asyncio

async def async_view(request):
    await asyncio.sleep(1)
    return HttpResponse('Hello async!')

# Async class-based view
from django.views import View

class AsyncView(View):
    async def get(self, request):
        await some_async_operation()
        return HttpResponse('Async response')
```

## Internationalization (i18n)

Django has comprehensive i18n support:

```python
from django.utils.translation import gettext_lazy as _

class MyModel(models.Model):
    title = models.CharField(_('title'), max_length=100)

# In views
from django.utils.translation import gettext as _
message = _('Hello, world!')

# Template
{% load i18n %}
{% trans "Welcome" %}
```

Locale files in `django/conf/locale/` for 100+ languages.

## Common Patterns and Idioms

### Defensive Attribute Access

When working with expressions in the ORM that may not be fully resolved:

```python
# Pattern 1: hasattr check with fallback
output_field = (
    self.lhs.output_field if hasattr(self.lhs, "output_field") else None
)

# Pattern 2: getattr with default
output_field = getattr(self.lhs, "output_field", None)
```

**When to use:**
- Working with F() expressions (don't have `output_field` until resolved)
- Any expression that may be in an intermediate state
- Common in `django/db/models/lookups.py` and `expressions.py`

**Why it matters:**
- F() and other expressions are lazily evaluated
- Accessing `output_field` before resolution raises `AttributeError`
- Always check existence before accessing optional attributes

### Lazy Objects

```python
from django.utils.functional import lazy, cached_property

# Lazy evaluation
lazy_value = lazy(expensive_function, str)()

# Cached property
class MyClass:
    @cached_property
    def expensive_property(self):
        return expensive_computation()
```

### Signal Usage

```python
from django.db.models.signals import pre_save, post_save
from django.dispatch import receiver

@receiver(post_save, sender=MyModel)
def handle_save(sender, instance, created, **kwargs):
    if created:
        # Do something when object is created
        pass
```

### Custom Managers and QuerySets

```python
class PublishedQuerySet(models.QuerySet):
    def published(self):
        return self.filter(status='published')

class ArticleManager(models.Manager):
    def get_queryset(self):
        return PublishedQuerySet(self.model, using=self._db)

    def published(self):
        return self.get_queryset().published()

class Article(models.Model):
    objects = ArticleManager()
```

### Context Processors

```python
# myapp/context_processors.py
def site_settings(request):
    return {
        'SITE_NAME': 'My Site',
        'SITE_VERSION': '1.0',
    }

# settings.py
TEMPLATES = [{
    'OPTIONS': {
        'context_processors': [
            'myapp.context_processors.site_settings',
        ],
    },
}]
```

## Key Files to Understand

When working on Django, these files are particularly important:

### Core Infrastructure

- `django/__init__.py`: Version and setup function
- `django/conf/global_settings.py`: Default settings
- `django/apps/registry.py`: App registry implementation
- `django/core/management/__init__.py`: Management command system

### ORM

- `django/db/models/base.py`: Model metaclass and base class
- `django/db/models/query.py`: QuerySet implementation
- `django/db/models/manager.py`: Manager implementation
- `django/db/backends/base/base.py`: Database backend base

### Views and URLs

- `django/views/generic/base.py`: Generic view base classes
- `django/urls/resolvers.py`: URL resolution
- `django/http/request.py`: HttpRequest implementation
- `django/http/response.py`: HttpResponse implementation

### Forms

- `django/forms/forms.py`: Form base classes
- `django/forms/models.py`: ModelForm implementation
- `django/forms/fields.py`: Form field implementations

### Templates

- `django/template/base.py`: Template engine core
- `django/template/context.py`: Template context
- `django/template/defaulttags.py`: Built-in template tags

## Documentation

Django has extensive documentation:

- Official docs: https://docs.djangoproject.com/
- Source docs: `docs/` directory (reStructuredText)
- Inline documentation: Comprehensive docstrings

### Building Documentation Locally

```bash
cd docs
pip install -r requirements.txt
make html
```

## Release Process

Django follows a time-based release schedule:

- Major releases every 8 months (e.g., 5.0, 5.1, 5.2)
- LTS (Long Term Support) every 2 years
- Security releases as needed

Version format: `MAJOR.MINOR.PATCH`

## Resources for Contributors

### Official Resources

- Contributing guide: https://docs.djangoproject.com/en/dev/internals/contributing/
- Code of Conduct: https://www.djangoproject.com/conduct/
- Django Discord: https://chat.djangoproject.com
- Django Forum: https://forum.djangoproject.com/

### Learning Resources

- Django tutorials in `docs/intro/`
- Topic guides in `docs/topics/`
- Reference documentation in `docs/ref/`
- How-to guides in `docs/howto/`

## Tips for AI Assistants

When working with Django code:

1. **Always check existing patterns**: Django has established patterns for most tasks
2. **Respect backward compatibility**: Django maintains strong backward compatibility
3. **Follow the test-driven approach**: Write tests first, then implementation
4. **Use appropriate abstractions**: Don't reinvent built-in functionality
5. **Consider database compatibility**: Code should work across all supported backends
6. **Think about security**: Always consider XSS, CSRF, SQL injection implications
7. **Document clearly**: Django values clear documentation and docstrings
8. **Check deprecation warnings**: Follow deprecation policy for removing features
9. **Internationalization**: Use translation functions for user-facing strings
10. **Performance matters**: Consider query optimization and caching
11. **Read the PR/issue description carefully**: Understand the EXACT scope before making changes
12. **Be surgical with documentation changes**: Only modify what's mentioned; don't add unrelated changes

### Common Gotchas

- Models require migrations after field changes
- QuerySets are lazy - they don't hit the database until evaluated
- Template context is isolated - use context processors for global data
- Middleware order matters - security middleware should be early
- Static files require `collectstatic` in production
- Time zones: Always use timezone-aware datetimes
- Model signals can cause infinite loops if not careful
- Database transactions: Be aware of atomic blocks and rollback
- F() expressions don't have `output_field` until resolved - always check with `hasattr()` before accessing

### File Change Patterns

When implementing features, typical file changes include:

1. **New model field**:
   - Update model in `django/db/models/fields/`
   - Add migration operation
   - Update form field if needed
   - Add tests in `tests/model_fields/`

2. **New view feature**:
   - Update view in `django/views/generic/`
   - Add URL pattern example
   - Update tests in `tests/generic_views/`
   - Document in `docs/ref/class-based-views/`

3. **ORM enhancement**:
   - Update `django/db/models/query.py` or related
   - Add backend support if needed
   - Comprehensive tests in `tests/queries/` or similar
   - Document in `docs/ref/models/`

4. **Bug fixes**:
   - Update code in `django/db/models/` or related
   - Add focused test(s) in existing `tests/` directories
   - **NO** CHANGES.md or FIX_SUMMARY.md files
   - **NO** standalone validation scripts
   - Keep changes minimal - just fix and test

5. **Documentation-only changes**:
   - Often target specific doc files mentioned in the ticket/PR
   - May remove obsolete messages after feature changes
   - Typically very surgical - only change what's mentioned in the issue
   - Common locations: `docs/intro/`, `docs/topics/`, `docs/ref/`

## Version-Specific Notes

### Django 6.1 (Current Development)

- Python 3.12+ required
- Development status: Pre-Alpha
- Building on async improvements
- Enhanced type hints
- Modern Python features (PEP 448 unpacking, etc.)

### Migration from Earlier Versions

When working on code that may affect older versions:
- Check deprecation timeline in RemovedInDjango70Warning
- Maintain compatibility with supported Python versions
- Follow the deprecation policy for breaking changes

---

**Last Updated**: Based on Django 6.1.0-alpha (main branch)

This guide should help AI assistants understand Django's architecture, conventions, and best practices when assisting with development tasks or implementing new features.
