# Text Input Entry for Filament

A Filament plugin that provides an editable text input entry component for infolists. This component allows users to edit text fields directly in the infolist view without navigating to an edit page.

![Black and White Simple Minimalist Pitch Deck Marketing Presentation](https://github.com/user-attachments/assets/6685eae7-c376-45ab-ac32-f28a6fd819a6)

## Features

- ✨ Inline editing directly in infolist views
- 🎨 Customizable border styling
- ✅ Built-in validation support
- 🔔 Instant success notifications
- 🎯 Icon support (before/after)
- 🌈 Color customization
- �� Responsive design
- 🔄 **Compatible with Filament v3, v4, and v5**

## Requirements

- PHP 8.1+
- Filament 3.0+, 4.0+, or 5.0+
- Laravel 10+

## Installation

You can install the package via Composer:

```bash
composer require anish/text-input-entry
```

## Setup

### 1. Install Assets

After installing the package, publish and build the assets:

```bash
npm install
npm run build
```

### 2. Use the Trait in Your View Page

Add the `TextInputEntryTrait` to your Filament resource's view page:

```php
<?php

namespace App\Filament\Resources\Users\Pages;

use Filament\Resources\Pages\ViewRecord;
use Anish\TextInputEntry\Traits\TextInputEntryTrait;

class ViewUser extends ViewRecord
{
    use TextInputEntryTrait;

    protected static string $resource = UserResource::class;
}
```

### 3. Use the Component in Your Infolist

Use `TextInputEntry` in your infolist schema:

```php
<?php

namespace App\Filament\Resources\Users\Schemas;

use Filament\Schemas\Schema;
use Anish\TextInputEntry\Infolists\Components\TextInputEntry;
use Filament\Support\Enums\TextSize;
use Illuminate\Support\Facades\Auth;

class UserInfolist
{
    public static function configure(Schema $schema): Schema
    {
        return $schema
            ->components([
                TextInputEntry::make('name')
                    ->editable(true)
                    ->size(TextSize::Large)
                    ->rules(['required', 'string', 'max:255'])
                    ->border(true),

                TextInputEntry::make('email')
                    ->editable(Auth::user()->can('update email'))
                    ->label('Email address')
                    ->rules(['required', 'email'])
                    ->border(true),
            ]);
    }
}
```

## Usage

### Basic Usage

```php
TextInputEntry::make('field_name')
    ->editable(true)
    ->border(true)
```

### With Validation

```php
TextInputEntry::make('email')
    ->rules(['required', 'email', 'max:255'])
    ->editable(true)
```

### With Custom Update Callback

```php
TextInputEntry::make('name')
    ->updateStateUsing(function ($value, $record) {
        $record->name = $value;
        $record->save();

        // Custom logic here
        Log::info("Name updated to: {$value}");
    })
```

### Without Border

```php
TextInputEntry::make('name')
    ->border(false) // Hides the border, shows as plain text until focused
```

### With Icon

```php
TextInputEntry::make('email')
    ->icon('heroicon-o-envelope')
    ->iconPosition('before')
```

### Conditional Editing

```php
TextInputEntry::make('name')
    ->editable(fn ($record) => auth()->user()->can('update', $record))
```

## Available Methods

- `editable(bool|Closure $editable)` - Enable/disable editing
- `border(bool|Closure $showBorder)` - Show/hide border (default: true)
- `rules(array|string|Closure $rules)` - Validation rules
- `updateStateUsing(Closure $callback)` - Custom update callback
- `size(TextSize $size)` - Text size (Small, Medium, Large)
- `color(string|array $color)` - Text color
- `icon(string $icon)` - Icon name
- `iconPosition(string $position)` - Icon position ('before' or 'after')
- `placeholder(string $placeholder)` - Input placeholder

## Compatibility

This package supports:

- ✅ Filament v3.0+
- ✅ Filament v4.0+
- ✅ Filament v5.0+

The package automatically detects and works with all supported versions.

## License

MIT

## Author

anishregmi17
