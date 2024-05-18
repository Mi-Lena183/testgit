1.	ТЕСТ


**Модифицированный бот:**

```javascript
const { Telegraf } = require('telegraf');
const axios = require('axios');

const TOKEN = 'YOUR_BOT_TOKEN';
const SPOONACULAR_API_KEY = 'YOUR_SPOONACULAR_API_KEY';
const SPOONACULAR_API_URL = 'https://api.spoonacular.com';

const bot = new Telegraf(TOKEN);

bot.start((ctx) => ctx.reply('Welcome to Recipe Bot! Send /random to get a random recipe or /recipes to see a list of available recipes.'));

// Authentication command handler
const authHandler = (ctx) => {
    const authCode = ctx.message.text.split('/auth ')[1];
    if (authCode) {
      ctx.reply(`Authentication successful! Entered code: ${authCode}`);
    } else {
      ctx.reply('Invalid authorization code. Please try again.');
    }
};

bot.command('auth', authHandler);

// Registration command handler
const registerHandler = (ctx) => {
    const userName = ctx.message.text.split('/register ')[1];
    if (userName) {
      ctx.reply(`User ${userName} registered successfully!`);
    } else {
      ctx.reply('Invalid registration format. Please use /register <username>');
    }
};

bot.command('register', registerHandler);

// Other command handlers can be defined similarly...

// Launch the bot
bot.launch();

module.exports = { bot, authHandler, registerHandler };
```

тесты для команды `/auth`:

**Тесты:**

```javascript
const { bot, authHandler } = require('../index');  // Импортируем бот и обработчик команды

describe('/auth command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should authorize user with correct key', () => {
        const ctx = {
            message: {
                text: '/auth secret'
            },
            reply: jest.fn()
        };
☺
        authHandler(ctx);
        expect(ctx.reply).toHaveBeenCalledWith('Authentication successful! Entered code: secret');
    });

    it('should not authorize user with incorrect key', () => {
        const ctx = {
            message: {
                text: '/auth'
            },
            reply: jest.fn()
        };

        authHandler(ctx);
        expect(ctx.reply).toHaveBeenCalledWith('Invalid authorization code. Please try again.');
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

ТЕСТ для команды `/register`:

```javascript
const { bot, registerHandler } = require('../index');  // Импортируем бот и обработчик команды

describe('/register command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should register user with correct format', () => {
        const ctx = {
            message: {
                text: '/register user123'
            },
            reply: jest.fn()
        };

        registerHandler(ctx);
        expect(ctx.reply).toHaveBeenCalledWith('User user123 registered successfully!');
    });

    it('should not register user with incorrect format', () => {
        const ctx = {
            message: {
                text: '/register'
            },
            reply: jest.fn()
        };

        registerHandler(ctx);
        expect(ctx.reply).toHaveBeenCalledWith('Invalid registration format. Please use /register <username>');
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

Эти тесты проверяют корректность выполнения команд, проверяя, как бота отвечает на различные входные сообщения. 


2.	ТЕСТ

Для создания тестов для вашего бота на основе предоставленного кода, будем использовать библиотеку `jest`. Вот как можно написать тесты для различных команд вашего бота, используя структуру, аналогичную предложенному примеру.

Модификация кода бота для экспорта обработчиков команд

Модифицируем код бота, чтобы экспортировать обработчики команд.

```javascript
const { Telegraf } = require('telegraf');
const axios = require('axios');

const TOKEN = 'YOUR_BOT_TOKEN';
const SPOONACULAR_API_KEY = 'YOUR_SPOONACULAR_API_KEY';
const SPOONACULAR_API_URL = 'https://api.spoonacular.com';

const bot = new Telegraf(TOKEN);

bot.start((ctx) => ctx.reply('Welcome to Recipe Bot! Send /random to get a random recipe or /recipes to see a list of available recipes.'));

// Authentication command handler
const authHandler = (ctx) => {
    const authCode = ctx.message.text.split('/auth ')[1];
    if (authCode) {
      ctx.reply(`Authentication successful! Entered code: ${authCode}`);
    } else {
      ctx.reply('Invalid authorization code. Please try again.');
    }
};

bot.command('auth', authHandler);

// Registration command handler
const registerHandler = (ctx) => {
    const userName = ctx.message.text.split('/register ')[1];
    if (userName) {
      ctx.reply(`User ${userName} registered successfully!`);
    } else {
      ctx.reply('Invalid registration format. Please use /register <username>');
    }
};

bot.command('register', registerHandler);

// Other command handlers can be defined similarly...

// Help command handler
const helpHandler = (ctx) => {
    const helpMessage = `Available commands:
  /random - Get a random recipe
  /recipes - See a list of available recipes
  /cuisine <cuisine_type> - Get a recipe by cuisine type
  /ingredient <ingredient> - Get a recipe by ingredient
  /auth <authorization_code> - Authenticate with the bot
  /register <username> - Register with the bot
  /unknown - Handle unknown commands
  /suggest - Suggest a new feature
  /support - Get support
  /about - Get information about the bot
  /start - Start the bot
  /help - Display this help message`;
    ctx.reply(helpMessage);
};

bot.command('help', helpHandler);

// Export bot and handlers for testing
module.exports = { bot, authHandler, registerHandler, helpHandler };
```


Тесты для команд `/auth`, `/register` и `/help`.

**Тесты для команды `/auth`:**

```javascript
const { bot, authHandler } = require('../index');

describe('/auth command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should authorize user with correct key', () => {
        const ctx = {
            message: {
                text: '/auth secret'
            },
            reply: jest.fn()
        };

        authHandler(ctx);
        expect(ctx.reply).toHaveBeenCalledWith('Authentication successful! Entered code: secret');
    });

    it('should not authorize user with incorrect key', () => {
        const ctx = {
            message: {
                text: '/auth'
            },
            reply: jest.fn()
        };

        authHandler(ctx);
        expect(ctx.reply).toHaveBeenCalledWith('Invalid authorization code. Please try again.');
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

**Тесты для команды `/register`:**

```javascript
const { bot, registerHandler } = require('../index');

describe('/register command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should register user with correct format', () => {
        const ctx = {
            message: {
                text: '/register user123'
            },
            reply: jest.fn()
        };

        registerHandler(ctx);
        expect(ctx.reply).toHaveBeenCalledWith('User user123 registered successfully!');
    });

    it('should not register user with incorrect format', () => {
        const ctx = {
            message: {
                text: '/register'
            },
            reply: jest.fn()
        };

        registerHandler(ctx);
        expect(ctx.reply).toHaveBeenCalledWith('Invalid registration format. Please use /register <username>');
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

**Тесты для команды `/help`:**

```javascript
const { bot, helpHandler } = require('../index');

describe('/help command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should display help message', () => {
        const ctx = {
            message: {
                text: '/help'
            },
            reply: jest.fn()
        };

        helpHandler(ctx);
        const helpMessage = `Available commands:
  /random - Get a random recipe
  /recipes - See a list of available recipes
  /cuisine <cuisine_type> - Get a recipe by cuisine type
  /ingredient <ingredient> - Get a recipe by ingredient
  /auth <authorization_code> - Authenticate with the bot
  /register <username> - Register with the bot
  /unknown - Handle unknown commands
  /suggest - Suggest a new feature
  /support - Get support
  /about - Get information about the bot
  /start - Start the bot
  /help - Display this help message`;
        expect(ctx.reply).toHaveBeenCalledWith(helpMessage);
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

Запуск тестов

```bash
npm install --save-dev jest
npm test
```

Эти тесты проверяют корректность выполнения команд бота и ответов на них. 

3.	ТЕСТ 



**Тесты для команды `/random`:**

```javascript
const { bot } = require('../index');
const axios = require('axios');

jest.mock('axios');

describe('/random command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should return a random recipe', async () => {
        const ctx = {
            message: {
                text: '/random'
            },
            replyWithMarkdown: jest.fn(),
            reply: jest.fn()
        };

        const recipe = {
            title: 'Test Recipe',
            sourceName: 'Test Source',
            sourceUrl: 'http://test.com',
            instructions: 'Test instructions',
            image: 'http://test.com/image.jpg'
        };

        axios.get.mockResolvedValue({ data: { recipes: [recipe] } });

        const randomHandler = bot.command('/random');
        await randomHandler(ctx);

        const recipeMessage = `
🍽 *Test Recipe*
📷 [Test Source](http://test.com)
📝 - Test instructions
📷 [http://test.com/image.jpg](http://test.com/image.jpg)
`;

        expect(ctx.replyWithMarkdown).toHaveBeenCalledWith(recipeMessage);
    });

    it('should handle errors gracefully', async () => {
        const ctx = {
            message: {
                text: '/random'
            },
            reply: jest.fn()
        };

        axios.get.mockRejectedValue(new Error('Test error'));

        const randomHandler = bot.command('/random');
        await randomHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith('Sorry, something went wrong. Please try again later.');
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

**Тесты для команды `/recipes`:**

```javascript
const { bot } = require('../index');

describe('/recipes command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should list available recipes', () => {
        const ctx = {
            message: {
                text: '/recipes'
            },
            reply: jest.fn()
        };

        const recipes = [
            'Italian',
            'Mexican',
            'Chinese',
            'Indian',
            'Thai',
            'Japanese',
            'Vegan',
            'Vegetarian',
            'Gluten-free',
            'Desserts'
        ];
        const recipeList = recipes.join(', ');

        const recipesHandler = bot.command('/recipes');
        recipesHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith(`Available recipes: ${recipeList}`);
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

**Тесты для команды `/cuisine`:**

```javascript
const { bot } = require('../index');
const axios = require('axios');

jest.mock('axios');

describe('/cuisine command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should return a recipe for a valid cuisine type', async () => {
        const ctx = {
            message: {
                text: '/cuisine italian'
            },
            replyWithMarkdown: jest.fn(),
            reply: jest.fn()
        };

        const recipe = {
            title: 'Test Italian Recipe',
            sourceName: 'Test Source',
            sourceUrl: 'http://test.com',
            instructions: 'Test instructions',
            image: 'http://test.com/image.jpg'
        };

        axios.get.mockResolvedValue({ data: { results: [recipe] } });

        const cuisineHandler = bot.command('/cuisine');
        await cuisineHandler(ctx);

        const recipeMessage = `
🍽 *Test Italian Recipe*
📷 [Test Source](http://test.com)
📝 - Test instructions
📷 [http://test.com/image.jpg](http://test.com/image.jpg)
`;

        expect(ctx.replyWithMarkdown).toHaveBeenCalledWith(recipeMessage);
    });

    it('should handle invalid cuisine type', async () => {
        const ctx = {
            message: {
                text: '/cuisine invalid'
            },
            reply: jest.fn()
        };

        const cuisineHandler = bot.command('/cuisine');
        await cuisineHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith('Sorry, that cuisine type is not available. Please try again.');
    });

    it('should handle errors gracefully', async () => {
        const ctx = {
            message: {
                text: '/cuisine italian'
            },
            reply: jest.fn()
        };

        axios.get.mockRejectedValue(new Error('Test error'));

        const cuisineHandler = bot.command('/cuisine');
        await cuisineHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith('Sorry, something went wrong. Please try again later.');
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

**Тесты для команды `/ingredient`:**

```javascript
const { bot } = require('../index');
const axios = require('axios');

jest.mock('axios');

describe('/ingredient command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should return a recipe for a valid ingredient', async () => {
        const ctx = {
            message: {
                text: '/ingredient chicken'
            },
            replyWithMarkdown: jest.fn(),
            reply: jest.fn()
        };

        const recipe = {
            title: 'Test Chicken Recipe',
            sourceName: 'Test Source',
            sourceUrl: 'http://test.com',
            instructions: 'Test instructions',
            image: 'http://test.com/image.jpg'
        };

        axios.get.mockResolvedValue({ data: { results: [recipe] } });

        const ingredientHandler = bot.command('/ingredient');
        await ingredientHandler(ctx);

        const recipeMessage = `
🍽 *Test Chicken Recipe*
📷 [Test Source](http://test.com)
📝 - Test instructions
📷 [http://test.com/image.jpg](http://test.com/image.jpg)
`;

        expect(ctx.replyWithMarkdown).toHaveBeenCalledWith(recipeMessage);
    });

    it('should handle errors gracefully', async () => {
        const ctx = {
            message: {
                text: '/ingredient chicken'
            },
            reply: jest.fn()
        };

        axios.get.mockRejectedValue(new Error('Test error'));

        const ingredientHandler = bot.command('/ingredient');
        await ingredientHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith('Sorry, something went wrong. Please try again later.');
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

bash
npm test

4.	ТЕСТ


### Экспорт обработчиков команд из файла `index.js`

В файле `index.js` нужно экспортировать обработчики команд:

```javascript
const { Telegraf } = require('telegraf');
const axios = require('axios');

const TOKEN = 'your-telegram-token';
const SPOONACULAR_API_KEY = 'your-spoonacular-api-key';
const SPOONACULAR_API_URL = 'https://api.spoonacular.com';

const bot = new Telegraf(TOKEN);

const authHandler = (ctx) => {
    const authCode = ctx.message.text.split('/auth ')[1];
    if (true) {
        ctx.reply(`Authentication successful! Entered code: ${authCode}`);
    } else {
        ctx.reply('Invalid authorization code. Please try again.');
    }
};

const registerHandler = (ctx) => {
    const userName = ctx.message.text.split('/register ')[1];
    if (userName) {
        ctx.reply(`User ${userName} registered successfully!`);
    } else {
        ctx.reply('Invalid registration format. Please use /register <username>');
    }
};

const randomHandler = async (ctx) => {
    try {
        const response = await axios.get(`${SPOONACULAR_API_URL}/recipes/random?apiKey=${SPOONACULAR_API_KEY}&number=1`);
        const recipe = response.data.recipes[0];
        const recipeMessage = `
🍽 *${recipe.title}*
📷 [${recipe.sourceName}](${recipe.sourceUrl})
📝 ${recipe.instructions.split('\n').map(line => `- ${line}`).join('\n')}
📷 [${recipe.image}](${recipe.image})
`;
        ctx.replyWithMarkdown(recipeMessage);
    } catch (error) {
        console.error(error);
        ctx.reply('Sorry, something went wrong. Please try again later.');
    }
};

const recipesHandler = (ctx) => {
    const recipes = [
        'Italian',
        'Mexican',
        'Chinese',
        'Indian',
        'Thai',
        'Japanese',
        'Vegan',
        'Vegetarian',
        'Gluten-free',
        'Desserts',
    ];
    const recipeList = recipes.join(', ');
    ctx.reply(`Available recipes: ${recipeList}`);
};

const cuisineHandler = async (ctx) => {
    const cuisine = ctx.message.text.split('/cuisine ')[1].toLowerCase();
    const cuisineTypes = {
        italian: 'Italian',
        mexican: 'Mexican',
        chinese: 'Chinese',
        indian: 'Indian',
        thai: 'Thai',
        japanese: 'Japanese',
        vegan: 'Vegan',
        vegetarian: 'Vegetarian',
        gluten_free: 'Gluten_free',
        desserts: 'Desserts',
    };
    if (!cuisineTypes[cuisine]) {
        ctx.reply('Sorry, that cuisine type is not available. Please try again.');
        return;
    }
    try {
        const response = await axios.get(`${SPOONACULAR_API_URL}/recipes/complexSearch?apiKey=${SPOONACULAR_API_KEY}&cuisine=${cuisineTypes[cuisine]}&number=1`);
        const recipe = response.data.results[0];
        const recipeMessage = `
🍽 *${recipe.title}*
📷 [${recipe.sourceName}](${recipe.sourceUrl})
📝 ${recipe.instructions.split('\n').map(line => `- ${line}`).join('\n')}
📷 [${recipe.image}](${recipe.image})
`;
        ctx.replyWithMarkdown(recipeMessage);
    } catch (error) {
        console.error(error);
        ctx.reply('Sorry, something went wrong. Please try again later.');
    }
};

const ingredientHandler = async (ctx) => {
    const ingredient = ctx.message.text.split('/ingredient ')[1].toLowerCase();
    try {
        const response = await axios.get(`${SPOONACULAR_API_URL}/recipes/complexSearch?apiKey=${SPOONACULAR_API_KEY}&includeIngredients=${ingredient}&number=1`);
        const recipe = response.data.results[0];
        const recipeMessage = `
🍽 *${recipe.title}*
📷 [${recipe.sourceName}](${recipe.sourceUrl})
📝 ${recipe.instructions.split('\n').map(line => `- ${line}`).join('\n')}
📷 [${recipe.image}](${recipe.image})
`;
        ctx.replyWithMarkdown(recipeMessage);
    } catch (error) {
        console.error(error);
        ctx.reply('Sorry, something went wrong. Please try again later.');
    }
};

bot.command('auth', authHandler);
bot.command('register', registerHandler);
bot.command('random', randomHandler);
bot.command('recipes', recipesHandler);
bot.command('cuisine', cuisineHandler);
bot.command('ingredient', ingredientHandler);

bot.launch();

module.exports = { bot, authHandler, registerHandler, randomHandler, recipesHandler, cuisineHandler, ingredientHandler };
```

### Тесты для команд бота


```javascript
const axios = require('axios');
const { bot, authHandler, registerHandler, randomHandler, recipesHandler, cuisineHandler, ingredientHandler } = require('../index');  // Импортируем бот и обработчики команд

jest.mock('axios');

describe('Bot commands', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    describe('/auth command', () => {
        it('should authenticate the user with a code', () => {
            const ctx = {
                message: {
                    text: '/auth secretcode'
                },
                reply: jest.fn()
            };

            authHandler(ctx);

            expect(ctx.reply).toHaveBeenCalledWith('Authentication successful! Entered code: secretcode');
        });
    });

    describe('/register command', () => {
        it('should register the user with a username', () => {
            const ctx = {
                message: {
                    text: '/register username'
                },
                reply: jest.fn()
            };

            registerHandler(ctx);

            expect(ctx.reply).toHaveBeenCalledWith('User username registered successfully!');
        });

        it('should return an error for invalid registration format', () => {
            const ctx = {
                message: {
                    text: '/register'
                },
                reply: jest.fn()
            };

            registerHandler(ctx);

            expect(ctx.reply).toHaveBeenCalledWith('Invalid registration format. Please use /register <username>');
        });
    });

    describe('/random command', () => {
        it('should return a random recipe', async () => {
            const ctx = {
                message: {
                    text: '/random'
                },
                replyWithMarkdown: jest.fn(),
                reply: jest.fn()
            };

            const recipe = {
                title: 'Test Recipe',
                sourceName: 'Test Source',
                sourceUrl: 'http://test.com',
                instructions: 'Test instructions',
                image: 'http://test.com/image.jpg'
            };

            axios.get.mockResolvedValue({ data: { recipes: [recipe] } });

            await randomHandler(ctx);

            const recipeMessage = `
🍽 *Test Recipe*
📷 [Test Source](http://test.com)
📝 - Test instructions
📷 [http://test.com/image.jpg](http://test.com/image.jpg)
`;

            expect(ctx.replyWithMarkdown).toHaveBeenCalledWith(recipeMessage);
        });

        it('should handle errors gracefully', async () => {
            const ctx = {
                message: {
                    text: '/random'
                },
                reply: jest.fn()
            };

            axios.get.mockRejectedValue(new Error('Test error'));

            await randomHandler(ctx);

            expect(ctx.reply).toHaveBeenCalledWith('Sorry, something went wrong. Please try again later.');
        });
    });

    describe('/recipes command', () => {
        it('should list available recipes', () => {
            const ctx = {
                message: {
                    text: '/recipes'
                },
                reply: jest.fn()
            };

            const recipes = [
                'Italian',
                'Mexican',
                'Chinese',
                'Indian',
                'Thai',
                'Japanese',
                'Vegan',
                'Vegetarian',
                'Gluten-free',
                'Desserts'
            ];
            const recipeList = recipes.join(', ');

            recipesHandler(ctx);

            expect(ctx.reply).toHaveBeenCalledWith(`Available recipes: ${recipeList}`);
        });
    });

    describe('/cuisine command', () => {
        it('should return a recipe for a valid cuisine type', async () => {
            const ctx = {
                message: {
                    text: '/cuisine italian'
                },
                replyWithMarkdown: jest.fn(),
                reply: jest.fn()
            };

            const recipe = {
                title: 'Test Italian Recipe',
                sourceName: 'Test Source',
                sourceUrl: 'http://test.com',
                instructions: 'Test instructions',
                image: 'http://test.com/image.jpg'
            };

            axios.get.mockResolvedValue({ data: { results: [recipe] } });

            await cuisineHandler(ctx);

            const recipeMessage = `
🍽 *Test Italian Recipe*
📷 [Test Source](http://test.com)
📝 - Test instructions
📷 [http://test.com/image.jpg](http://test

.com/image.jpg)
`;

            expect(ctx.replyWithMarkdown).toHaveBeenCalledWith(recipeMessage);
        });

        it('should handle invalid cuisine types', async () => {
            const ctx = {
                message: {
                    text: '/cuisine invalid'
                },
                reply: jest.fn()
            };

            await cuisineHandler(ctx);

            expect(ctx.reply).toHaveBeenCalledWith('Sorry, that cuisine type is not available. Please try again.');
        });
    });

    describe('/ingredient command', () => {
        it('should return a recipe for a valid ingredient', async () => {
            const ctx = {
                message: {
                    text: '/ingredient tomato'
                },
                replyWithMarkdown: jest.fn(),
                reply: jest.fn()
            };

            const recipe = {
                title: 'Test Tomato Recipe',
                sourceName: 'Test Source',
                sourceUrl: 'http://test.com',
                instructions: 'Test instructions',
                image: 'http://test.com/image.jpg'
            };

            axios.get.mockResolvedValue({ data: { results: [recipe] } });

            await ingredientHandler(ctx);

            const recipeMessage = `
🍽 *Test Tomato Recipe*
📷 [Test Source](http://test.com)
📝 - Test instructions
📷 [http://test.com/image.jpg](http://test.com/image.jpg)
`;

            expect(ctx.replyWithMarkdown).toHaveBeenCalledWith(recipeMessage);
        });

        it('should handle API errors gracefully', async () => {
            const ctx = {
                message: {
                    text: '/ingredient tomato'
                },
                reply: jest.fn()
            };

            axios.get.mockRejectedValue(new Error('Test error'));

            await ingredientHandler(ctx);

            expect(ctx.reply).toHaveBeenCalledWith('Sorry, something went wrong. Please try again later.');
        });
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

тесты для команд `/auth`, `/register`, `/random`, `/recipes`, `/cuisine`, и `/ingredient

5 ТЕСТ

тестирование команд: `/auth`, `/register`, `/random`, `/recipes`, `/cuisine`, и `/ingredient`. 

Сначала создадим тест для команды `/auth`.

### Тест для команды `/auth`
```javascript
const { bot, authHandler } = require('../index'); 
 // Импортируем бот и обработчик команды

describe('/auth command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should authorize user with provided code', () => {
        const ctx = {
            message: {
                text: '/auth mycode'
            },
            reply: jest.fn()
        };

        authHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith('Authentication successful! Entered code: mycode');
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

### Тест для команды `/register`
```javascript
const { bot, registerHandler } = require('../index');  
// Импортируем бот и обработчик команды

describe('/register command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should register user with provided username', () => {
        const ctx = {
            message: {
                text: '/register testuser'
            },
            reply: jest.fn()
        };

        registerHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith('User testuser registered successfully!');
    });

    it('should handle missing username', () => {
        const ctx = {
            message: {
                text: '/register'
            },
            reply: jest.fn()
        };

        registerHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith('Invalid registration format. Please use /register <username>');
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

### Тест для команды `/random`
```javascript
const { bot, randomHandler } = require('../index');  
// Импортируем бот и обработчик команды

describe('/random command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should return a random recipe', async () => {
        const ctx = {
            message: {
                text: '/random'
            },
            replyWithMarkdown: jest.fn(),
            reply: jest.fn()
        };

        const recipe = {
            title: 'Test Recipe',
            sourceName: 'Test Source',
            sourceUrl: 'http://test.com',
            instructions: 'Test instructions',
            image: 'http://test.com/image.jpg'
        };

        jest.spyOn(axios, 'get').mockResolvedValue({ data: { recipes: [recipe] } });

        await randomHandler(ctx);

        const recipeMessage = `
🍽 *Test Recipe*
📷 [Test Source](http://test.com)
📝 - Test instructions
📷 [http://test.com/image.jpg](http://test.com/image.jpg)
`;

        expect(ctx.replyWithMarkdown).toHaveBeenCalledWith(recipeMessage);
    });

    afterEach(() => {
        bot.stop('test');
        axios.get.mockRestore();
    });
});
```

### Тест для команды `/recipes`
```javascript
const { bot, recipesHandler } = require('../index'); 
 // Импортируем бот и обработчик команды

describe('/recipes command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should list available recipes', () => {
        const ctx = {
            message: {
                text: '/recipes'
            },
            reply: jest.fn()
        };

        recipesHandler(ctx);

        const recipeList = 'Available recipes: Italian, Mexican, Chinese, Indian, Thai, Japanese, Vegan, Vegetarian, Gluten-free, Desserts';

        expect(ctx.reply).toHaveBeenCalledWith(recipeList);
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

### Тест для команды `/cuisine`
```javascript
const { bot, cuisineHandler } = require('../index');  
// Импортируем бот и обработчик команды

describe('/cuisine command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should return a recipe for a valid cuisine type', async () => {
        const ctx = {
            message: {
                text: '/cuisine Italian'
            },
            replyWithMarkdown: jest.fn(),
            reply: jest.fn()
        };

        const recipe = {
            title: 'Test Italian Recipe',
            sourceName: 'Test Source',
            sourceUrl: 'http://test.com',
            instructions: 'Test instructions',
            image: 'http://test.com/image.jpg'
        };

        jest.spyOn(axios, 'get').mockResolvedValue({ data: { results: [recipe] } });

        await cuisineHandler(ctx);

        const recipeMessage = `
🍽 *Test Italian Recipe*
📷 [Test Source](http://test.com)
📝 - Test instructions
📷 [http://test.com/image.jpg](http://test.com/image.jpg)
`;

        expect(ctx.replyWithMarkdown).toHaveBeenCalledWith(recipeMessage);
    });

    it('should handle invalid cuisine types', async () => {
        const ctx = {
            message: {
                text: '/cuisine invalid'
            },
            reply: jest.fn()
        };

        await cuisineHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith('Sorry, that cuisine type is not available. Please try again.');
    });

    afterEach(() => {
        bot.stop('test');
        axios.get.mockRestore();
    });
});
```

### Тест для команды `/ingredient`
```javascript

const { bot, ingredientHandler } = require('../index');  
// Импортируем бот и обработчик команды

describe('/ingredient command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should return a recipe for a valid ingredient', async () => {
        const ctx = {
            message: {
                text: '/ingredient tomato'
            },
            replyWithMarkdown: jest.fn(),
            reply: jest.fn()
        };

        const recipe = {
            title: 'Test Tomato Recipe',
            sourceName: 'Test Source',
            sourceUrl: 'http://test.com',
            instructions: 'Test instructions',
            image: 'http://test.com/image.jpg'
        };

        jest.spyOn(axios, 'get').mockResolvedValue({ data: { results: [recipe] } });

        await ingredientHandler(ctx);

        const recipeMessage = `
🍽 *Test Tomato Recipe*
📷 [Test Source](http://test.com)
📝 - Test instructions
📷 [http://test.com/image.jpg](http://test.com/image.jpg)
`;

        expect(ctx.replyWithMarkdown).toHaveBeenCalledWith(recipeMessage);
    });

    it('should handle API errors gracefully', async () => {
        const ctx = {
            message: {
                text: '/ingredient tomato'
            },
            reply: jest.fn()
        };

        jest.spyOn(axios, 'get').mockRejectedValue(new Error('Test error'));

        await ingredientHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith('Sorry, something went wrong. Please try again later.');
    });

    afterEach(() => {
        bot.stop('test');
        axios.get.mockRestore();
    });
});
```

Эти тесты покрывают команды `/auth`, `/register`, `/random`, `/recipes`, `/cuisine`, и `/ingredient`. Они проверяют, что команды возвращают ожидаемые ответы и корректно обрабатывают ошибки. 

6 ТЕСТ


### Тест для команды `/about`
```javascript
const { bot, aboutHandler } = require('../index');  
// Импортируем бот и обработчик команды

describe('/about command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        bot.launch();
    });

    it('should return information about the bot', () => {
        const ctx = {
            message: {
                text: '/about'
            },
            reply: jest.fn()
        };

        aboutHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith('Recipe Bot is a bot that helps you find recipes based on cuisine type or ingredient.');
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

В этом тесте проверяется, что команда `/about` возвращает ожидаемую информацию о боте. Мы передаем объект `ctx`, который содержит имитацию сообщения пользователя, а также функцию `reply`, которая является имитацией функции ответа бота. После выполнения команды мы проверяем, что функция `reply` была вызвана с ожидаемым текстом.

7 ТЕСТ

Тест для команды `/support`:

```javascript
const { bot, supportHandler } = require('../index');  
// Импортируем бот и обработчик команды

describe('/support command', () => {
    beforeEach(() => {
        jest.spyOn(bot, 'launch').mockImplementation(() => {});
        jest.spyOn(bot, 'stop').mockImplementation(() => {});
        jest.spyOn(bot.telegram, 'sendMessage').mockImplementation(() => Promise.resolve());  // Мокируем sendMessage
        bot.launch();
    });

    it('should send a support request to admin', () => {
        const ctx = {
            message: {
                text: '/support help'
            },
            from: {
                username: 'testuser'
            },
            reply: jest.fn()
        };

        supportHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith('Ваш запрос на помощь отправлен администратору.');
// Проверка отправки сообщения админу
        expect(bot.telegram.sendMessage).toHaveBeenCalledWith(
            '851460416',
            'Пользователь @testuser запрашивает помощь: help'
        );
    });

    it('should handle empty support message', () => {
        const ctx = {
            message: {
                text: '/support'
            },
            from: {
                username: 'testuser'
            },
            reply: jest.fn()
        };

        supportHandler(ctx);

        expect(ctx.reply).toHaveBeenCalledWith('Этот запрос на помощь отправлен администратору.');
        // Проверка отправки сообщения админу
        expect(bot.telegram.sendMessage).toHaveBeenCalledWith(
            '851460416',
            'Пользователь @testuser запрашивает помощь: '
        );
    });

    afterEach(() => {
        bot.stop('test');
    });
});
```

Этот тест проверяет, что команда `/support` отправляет сообщение администратору с запросом на помощь. Мы создаем фиктивный контекст (ctx), содержащий текст запроса и информацию о пользователе, а затем проверяем, что функция ответа вызывается с правильным сообщением, и что функция `sendMessage` вызывается с ожидаемыми аргументами (идентификатором администратора и сообщением с запросом на помощь).



