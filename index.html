<?php

namespace App\Console\Commands;

use Closure;
use Exception;
use Illuminate\Console\Command;
use Illuminate\Filesystem\Filesystem;
use Illuminate\Support\Facades\Process;
use Laravel\Prompts as P;

class AppInstallCommand extends Command
{
    private array $config = [];

    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'app:install';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Get the application ready for use';

    public function __construct(
        private Filesystem $files,
    ) {
        parent::__construct();
    }

    /**
     * Execute the console command.
     *
     * @return int
     */
    public function handle(): int
    {
        try {
            P\warning('This command should be run only on a fresh Laravel installation.');
            P\warning('It will overwrite some files and might break your application.');

            if (!P\confirm('Do you want to continue?')) {
                P\info('Installation aborted.');

                return Command::SUCCESS;
            }

            P\clear();

            $this->prepare();

            P\progress(
                label: 'Initiating installtion process...',
                steps: $this->steps(),
                callback: function (Closure $callback, P\Progress $progress) {
                    call_user_func($callback($progress));
                },
            );

            P\info('Installation complete!');
            P\warning('Make `composer code:check` runs without errors before continuing.');

            return Command::SUCCESS;
        } catch (Exception $e) {
            P\error($e->getMessage());

            return Command::FAILURE;
        } finally {
            $this->files->delete(__FILE__);
        }
    }

    private function prepare(): void
    {
        $this->config = [
            'application_name' => $this->askForApplicationName(),
            'base_files' => $this->askForBaseFiles(),
            'tools' => $this->askForTools(),
            'composer' => $this->askForComposerInfo(),
            'move_tinker' => $this->askAboutMovingTinker(),
            'remove_frontend' => $this->askAboutRemovingFrontend(),
        ];
    }

    private function steps(): array
    {
        $steps = [
            $this->setApplicationNameInFiles(),
            $this->configureGitIgnore(),
            $this->configurePHPUnit(),
            $this->configureComposer(),
        ];

        if (in_array('CHANGELOG', $this->config['base_files'])) {
            $steps[] = $this->createChangelog();
        }

        if (in_array('README', $this->config['base_files'])) {
            $steps[] = $this->createReadme();
        }

        if (in_array('pint', $this->config['tools'])) {
            $steps[] = $this->installPint();
        }

        if (in_array('phpstan', $this->config['tools'])) {
            $steps[] = $this->installPHPStan();
        }

        if (in_array('phpcodesniffer', $this->config['tools'])) {
            $steps[] = $this->installPHPCodeSniffer();
        }

        if (in_array('infection', $this->config['tools'])) {
            $steps[] = $this->installInfection();
        }

        if (in_array('psl', $this->config['tools'])) {
            $steps[] = $this->installPSL();
        }

        if (in_array('safe', $this->config['tools'])) {
            $steps[] = $this->installSafe();
        }

        if ($this->config['move_tinker']) {
            $steps[] = $this->moveTinker();
        }

        if ($this->config['remove_frontend']) {
            $steps[] = $this->removeFrontend();
        }

        return $steps;
    }

    private function askForApplicationName(): string
    {
        $applicationName = P\text('Application Name', 'e.g. MyApp', validate: function ($value) {
            if (str($value)->replaceMatches('/[^a-zA-Z]/', '')->studly()->exactly($value)) {
                return null;
            }

            return 'Application name must be in StudlyCase containing only letters.';
        });

        P\clear();

        return $applicationName;
    }

    private function askForBaseFiles(): array
    {
        $baseFiles = P\multiselect(
            'Select the files you want created',
            ['CHANGELOG', 'README'],
            ['CHANGELOG', 'README'],
        );

        P\clear();

        return $baseFiles;
    }

    private function askForTools(): array
    {
        $tools = P\multiselect(
            'Select the tools you want installed',
            $tools = [
                'pint' => 'Pint',
                'phpstan' => 'PHP Stan',
                'phpcodesniffer' => 'PHP Code Sniffer',
                'infection' => 'Infection',
                'psl' => 'PHP Standard Library',
                'safe' => 'Safe',
            ],
            array_keys($tools),
            10,
        );

        P\clear();

        return $tools;
    }

    private function askForComposerInfo(): array
    {
        $composer = [
            'name' => P\text('Composer Name', 'e.g. myname/myapp', validate: function ($value): ?string {
                if (preg_match('#^[a-z0-9]([_.-]?[a-z0-9]+)*/[a-z0-9](([_.]|-{1,2})?[a-z0-9]+)*$#', $value) === 1) {
                    return null;
                }

                return 'Composer name must be in the format vendor/package.';
            }),
            'description' => P\text('Composer Description', 'e.g. My awesome app', required: true),
            'license' => P\text('Composer License', 'e.g. MIT', default: 'proprietary', required: true),
            'authors' => [
                [
                    'name' => P\text('Composer Author Name', 'e.g. John Doe', required: true),
                    'email' => P\text('Composer Author Email', 'e.g. john@doe.com', required: true, validate: 'email:rfc,strict'),
                    'role' => P\text('Composer Author Role', 'e.g. Maintainer', default: 'Maintainer', required: true),
                    'homepage' => P\text('Composer Author Homepage', 'e.g. https://johndoe.com', required: true, validate: 'url:https'),
                ],
            ],
        ];

        P\clear();

        return $composer;
    }

    private function askAboutMovingTinker(): bool
    {
        $confirm = P\confirm('Move Tinker to dev dependencies?', default: true);

        P\clear();

        return $confirm;
    }

    private function askAboutRemovingFrontend(): bool
    {
        $confirm = P\confirm('Remove frontend files?', default: true);

        P\clear();

        return $confirm;
    }

    private function setApplicationNameInFiles(): Closure
    {
        return function (P\Progress $progress): Closure {
            $progress->label('Setting application name in files...')->render();

            return function (): void {
                sleep(1);

                $applicationName = $this->config['application_name'];
                $lowercaseApplicationName = strtolower($applicationName);

                $replacements = [
                    '.env' => [
                        'APP_NAME=Laravel' => "APP_NAME=$applicationName",
                        'DB_DATABASE=laravel' => "DB_DATABASE=$applicationName",
                    ],
                    '.env.example' => [
                        'APP_NAME=Laravel' => "APP_NAME=$applicationName",
                        'DB_DATABASE=laravel' => "DB_DATABASE=$lowercaseApplicationName",
                    ],
                    'config/app.php' => [
                        "env('APP_NAME', 'Laravel')" => "env('APP_NAME', '$applicationName')",
                    ],
                    'config/cache.php' => [
                        "env('APP_NAME', 'laravel')" => "env('APP_NAME', '$lowercaseApplicationName')",
                    ],
                    'config/database.php' => [
                        "env('DB_DATABASE', 'laravel')" => "env('DB_DATABASE', '$lowercaseApplicationName')",
                        "env('APP_NAME', 'laravel')" => "env('APP_NAME', '$lowercaseApplicationName')",
                    ],
                    'config/logging.php' => [
                        "storage_path('logs/laravel.log')" => "storage_path('logs/$lowercaseApplicationName.log')",
                        "env('LOG_SLACK_USERNAME', 'Laravel Log')" => "env('LOG_SLACK_USERNAME', '$applicationName Log')",
                    ],
                    'config/session.php' => [
                        "env('APP_NAME', 'laravel')" => "env('APP_NAME', '$lowercaseApplicationName')",
                    ],
                ];

                foreach ($replacements as $file => $replacements) {
                    $this->replaceInFile($file, $replacements);
                }
            };
        };
    }

    private function configureGitIgnore(): Closure
    {
        return function (P\Progress $progress): Closure {
            $progress->label('Configuring .gitignore...')->render();

            return function (): void {
                P\spin(function (): void {
                    Process::run([
                        'curl',
                        '-s',
                        'https://raw.githubusercontent.com/aldemeery/laraset/refs/heads/master/.gitignore',
                        '-o',
                        '.gitignore',
                    ])->throw();
                }, 'Downloading .gitignore...');
            };
        };
    }

    private function configurePHPUnit(): Closure
    {
        return function (P\Progress $progress): Closure {
            $progress->label('Configuring PHPUnit...')->render();

            return function (): void {
                P\spin(function (): void {
                    Process::run([
                        'curl',
                        '-s',
                        'https://raw.githubusercontent.com/aldemeery/laraset/refs/heads/master/phpunit.xml',
                        '-o',
                        'phpunit.xml',
                    ])->throw();
                }, 'Downloading phpunit.xml...');
            };
        };
    }

    private function configureComposer(): Closure
    {
        return function (P\Progress $progress): Closure
        {
            $progress->label('Configuring composer.json...')->render();

            return function (): void {
                sleep(1);

                $config = json_decode($this->files->get('composer.json'), true);

                $composer = [
                    '$schema' => $config['$schema'] ?? 'https://getcomposer.org/schema.json',
                    'name' => $this->config['composer']['name'],
                    'type' => $config['type'] ?? 'project',
                    'description' => $this->config['composer']['description'],
                    'license' => $this->config['composer']['license'],
                    'authors' => $this->config['composer']['authors'],
                    'require' => $config['require'] ?? [],
                    'require-dev' => $config['require-dev'] ?? [],
                    'autoload' => $config['autoload'] ?? [],
                    'autoload-dev' => $config['autoload-dev'] ?? [],
                    'scripts' => [
                        ...($config['scripts'] ?? []),
                        ...$this->composerScripts(),
                    ],
                    'extra' => $config['extra'] ?? [],
                    'config' => $config['config'] ?? [],
                    'minimum-stability' => $config['minimum-stability'] ?? 'stable',
                    'prefer-stable' => $config['prefer-stable'] ?? true,
                ];

                $this->files->put('composer.json', json_encode($composer, JSON_PRETTY_PRINT | JSON_UNESCAPED_SLASHES));
            };
        };
    }

    private function createChangelog(): Closure
    {
        return function (P\Progress $progress): Closure
        {
            $progress->label('Creating CHANGELOG.md...')->render();

            return function (): void {
                sleep(1);

                $this->files->put(base_path('CHANGELOG.md'), <<<CHL
                # Changelog

                All notable changes to this project will be documented in this file.

                The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
                and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

                ## [Unreleased]

                CHL);
            };
        };
    }

    private function createReadme(): Closure
    {
        return function (P\Progress $progress): Closure
        {
            $progress->label('Creating README.md...')->render();

            return function (): void {
                sleep(1);

                $applicationName = $this->config['application_name'];

                $this->files->put(base_path('README.md'), <<<RDM
                # $applicationName

                RDM);
            };
        };
    }

    private function replaceInFile(string $file, array $replacements): void
    {
        $filePath = base_path($file);

        if ($this->files->exists($filePath)) {
            $contents = $this->files->get($filePath);

            foreach ($replacements as $search => $replace) {
                $contents = str_replace($search, $replace, $contents);
            }

            $this->files->put($filePath, $contents);
        }
    }

    private function installPint(): Closure
    {
        return function (P\Progress $progress): Closure {
            $progress->label('Installing Pint...')->render();

            return function (): void {
                P\spin(function (): void {
                    $this->composerRequire(['laravel/pint'], dev: true);

                    Process::run([
                        'curl',
                        '-s',
                        'https://raw.githubusercontent.com/aldemeery/laraset/refs/heads/master/pint.json',
                        '-o',
                        'pint.json',
                    ])->throw();
                }, 'Downloading Pint...');
            };
        };
    }

    private function installPHPStan(): Closure
    {
        return function (P\Progress $progress): Closure {
            $progress->label('Installing PHPStan...')->render();

            return function (): void {
                P\spin(function (): void {
                    $this->composerRequire([
                        'phpstan/phpstan',
                        'larastan/larastan',
                        'phpstan/phpstan-phpunit',
                        'phpstan/phpstan-mockery',
                    ], dev: true);

                    Process::run([
                        'curl',
                        '-s',
                        'https://raw.githubusercontent.com/aldemeery/laraset/refs/heads/master/phpstan.neon.dist',
                        '-o',
                        'phpstan.neon.dist',
                    ])->throw();
                }, 'Downloading PHPStan...');
            };
        };
    }

    private function installPHPCodeSniffer(): Closure
    {
        return function (P\Progress $progress): Closure {
            $progress->label('Installing PHP Code Sniffer...')->render();

            return function (): void {
                P\spin(function (): void {
                    $this->composerRequire(['squizlabs/php_codesniffer'], dev: true);

                    Process::run([
                        'curl',
                        '-s',
                        'https://raw.githubusercontent.com/aldemeery/laraset/refs/heads/master/phpcs.xml.dist',
                        '-o',
                        'phpcs.xml.dist',
                    ])->throw();
                }, 'Downloading PHP Code Sniffer...');
            };
        };
    }

    private function installInfection(): Closure
    {
        return function (P\Progress $progress): Closure {
            $progress->label('Installing Infection...')->render();

            return function (): void {
                P\spin(function (): void {
                    Process::run([
                        'composer',
                        'config',
                        'allow-plugins.infection/extension-installer',
                        'true',
                    ]);

                    $this->composerRequire(['infection/infection'], dev: true);

                    Process::run([
                        'curl',
                        '-s',
                        'https://raw.githubusercontent.com/aldemeery/laraset/refs/heads/master/infection.json.dist',
                        '-o',
                        'infection.json.dist',
                    ])->throw();
                }, 'Downloading Infection...');
            };
        };
    }

    private function installPSL(): Closure
    {
        return function (P\Progress $progress): Closure {
            $progress->label('Installing PHP Standard Library...')->render();

            return function (): void {
                P\spin(function (): void {
                    $this->composerRequire(['azjezz/psl']);

                    if (in_array('phpstan', $this->config['tools'])) {
                        $this->composerRequire(['php-standard-library/phpstan-extension'], dev: true);
                    }
                }, 'Downloading PHP Standard Library...');
            };
        };
    }

    private function installSafe(): Closure
    {
        return function (P\Progress $progress): Closure {
            $progress->label('Installing Safe...')->render();

            return function (): void {
                P\spin(function (): void {
                    $this->composerRequire(['thecodingmachine/safe']);

                    if (in_array('phpstan', $this->config['tools'])) {
                        $this->composerRequire(['thecodingmachine/phpstan-safe-rule'], dev: true);
                    }
                }, 'Downloading Safe...');
            };
        };
    }

    private function moveTinker(): Closure
    {
        return function (P\Progress $progress): Closure {
            $progress->label('Moving Tinker to Dev-Dependencies...')->render();

            return function (): void {
                P\spin(function (): void {
                    $this->composerRequire(['laravel/tinker'], dev: true);
                }, 'Moving...');
            };
        };
    }

    private function removeFrontend(): Closure
    {
        return function (P\Progress $progress): Closure {
            $progress->label('Removing frontend files...')->render();

            return function (): void {
                sleep(1);

                $applicationName = $this->config['application_name'];

                $this->files->delete([
                    'package.json',
                    'postcss.config.js',
                    'tailwind.config.js',
                    'vite.config.js',
                    'resources/views/welcome.blade.php',
                ]);

                $this->files->deleteDirectory('resources/css');
                $this->files->deleteDirectory('resources/js');

                $this->files->put('resources/views/.gitkeep', '');
                $this->files->put('routes/web.php', <<<PHP
                <?php

                use Illuminate\Support\Facades\Route;

                Route::get('/', fn (): array => ["$applicationName" => '0.1.0']);

                PHP);
            };
        };
    }

    private function composerRequire(array $packages, bool $dev = false): void
    {
        $command = ['composer', 'require', '--optimize-autoloader', '--sort-packages', '--no-interaction'];

        if ($dev) {
            $command[] = '--dev';
        }

        Process::run([...$command, ...$packages])->throw();
    }

    private function composerScripts(): array
    {
        return [
            ...(in_array('pint', $this->config['tools']) ? [
                'lint' => 'pint --test',
                'lint:fix' => 'pint',
                'lint:dirty' => 'pint --dirty --test',
                'lint:dirty:fix' => 'pint --dirty',
            ] : []),
            ...(in_array('phpcodesniffer', $this->config['tools']) ? [
                'sniff' => 'phpcs --extensions=php',
                'sniff:fix' => 'phpcbf --extensions=php',
            ] : []),
            ...(in_array('phpstan', $this->config['tools']) ? [
                'analyze:phpstan' => 'phpstan analyse --memory-limit=6G',
            ] : []),
            'test' => 'phpunit',
            ...(in_array('infection', $this->config['tools']) ? [
                'test:mutate' => [
                    "Composer\\Config::disableProcessTimeout",
                    "infection --threads=12"
                ],
            ] : []),
            'code:check' => [
                ...(in_array('pint', $this->config['tools']) ? ['@lint'] : []),
                ...(in_array('phpcodesniffer', $this->config['tools']) ? ['@sniff'] : []),
                ...(in_array('phpstan', $this->config['tools']) ? ['@analyze:phpstan'] : []),
                '@test',
                ...(in_array('infection', $this->config['tools']) ? ['@test:mutate'] : []),
            ],
        ];
    }
}
