# Libraries, Frameworks, and Tools for AI Chatbot Development

This section provides a comprehensive overview of the libraries, frameworks, and tools you'll need to build AI chatbots using .NET C# and JavaScript. Each recommendation includes installation instructions, key features, and code examples to help you get started quickly.

## .NET C# Libraries and Frameworks

### 1. LLM Integration Libraries

#### OpenAI API Client for .NET

The OpenAI API client allows you to interact with OpenAI's models (GPT-4, GPT-3.5, etc.) from your .NET applications.

**Installation:**
```bash
dotnet add package OpenAI
```

**Key Features:**
- Complete API coverage for OpenAI services
- Async support for all operations
- Strongly typed models for request and response handling
- Customizable HTTP client with retry logic

**Code Example - Text Generation:**
```csharp
using OpenAI_API;
using OpenAI_API.Completions;

public class OpenAiService
{
    private readonly OpenAIAPI _api;
    
    public OpenAiService(string apiKey)
    {
        _api = new OpenAIAPI(apiKey);
    }
    
    public async Task<string> GenerateTextAsync(string prompt)
    {
        var completionRequest = new CompletionRequest
        {
            Prompt = prompt,
            Model = "gpt-3.5-turbo-instruct",
            MaxTokens = 500,
            Temperature = 0.7,
            TopP = 1.0,
            FrequencyPenalty = 0.0,
            PresencePenalty = 0.0
        };
        
        var result = await _api.Completions.CreateCompletionAsync(completionRequest);
        return result.Completions[0].Text;
    }
}
```

**Code Example - Chat Completion:**
```csharp
using OpenAI_API;
using OpenAI_API.Chat;

public class OpenAiChatService
{
    private readonly OpenAIAPI _api;
    
    public OpenAiChatService(string apiKey)
    {
        _api = new OpenAIAPI(apiKey);
    }
    
    public async Task<string> GetChatResponseAsync(string userMessage, List<ChatMessage> history = null)
    {
        var chatRequest = new ChatRequest();
        
        if (history == null || !history.Any())
        {
            chatRequest.Messages = new List<ChatMessage>
            {
                new ChatMessage(ChatMessageRole.System, "You are a helpful assistant."),
                new ChatMessage(ChatMessageRole.User, userMessage)
            };
        }
        else
        {
            history.Add(new ChatMessage(ChatMessageRole.User, userMessage));
            chatRequest.Messages = history;
        }
        
        var result = await _api.Chat.CreateChatCompletionAsync(chatRequest);
        var responseMessage = result.Choices[0].Message;
        
        if (history != null)
        {
            history.Add(responseMessage);
        }
        
        return responseMessage.Content;
    }
}
```

#### Azure OpenAI SDK for .NET

For organizations using Azure OpenAI services, Microsoft provides a dedicated SDK.

**Installation:**
```bash
dotnet add package Azure.AI.OpenAI
```

**Key Features:**
- Seamless integration with Azure OpenAI services
- Azure Active Directory authentication
- Compliance with Azure security standards
- Support for Azure-specific features

**Code Example:**
```csharp
using Azure;
using Azure.AI.OpenAI;

public class AzureOpenAiService
{
    private readonly OpenAIClient _client;
    private readonly string _deploymentName;
    
    public AzureOpenAiService(string endpoint, string apiKey, string deploymentName)
    {
        _client = new OpenAIClient(new Uri(endpoint), new AzureKeyCredential(apiKey));
        _deploymentName = deploymentName;
    }
    
    public async Task<string> GetChatCompletionAsync(string userMessage)
    {
        var chatCompletionsOptions = new ChatCompletionsOptions
        {
            Messages =
            {
                new ChatMessage(ChatRole.System, "You are a helpful assistant."),
                new ChatMessage(ChatRole.User, userMessage)
            },
            MaxTokens = 500,
            Temperature = 0.7,
            DeploymentName = _deploymentName
        };
        
        Response<ChatCompletions> response = await _client.GetChatCompletionsAsync(chatCompletionsOptions);
        ChatCompletions completions = response.Value;
        
        return completions.Choices[0].Message.Content;
    }
}
```

#### Semantic Kernel

Microsoft's Semantic Kernel is a powerful SDK that integrates LLMs with conventional programming.

**Installation:**
```bash
dotnet add package Microsoft.SemanticKernel
```

**Key Features:**
- Combines symbolic AI (traditional code) with LLMs
- Plugin architecture for extending functionality
- Memory management for conversations
- Planning capabilities for complex tasks

**Code Example:**
```csharp
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.Connectors.AI.OpenAI;

public class SemanticKernelService
{
    private readonly IKernel _kernel;
    
    public SemanticKernelService(string apiKey)
    {
        // Create a kernel builder
        var builder = Kernel.CreateBuilder();
        
        // Add OpenAI service
        builder.AddOpenAIChatCompletion("gpt-4", apiKey);
        
        // Build the kernel
        _kernel = builder.Build();
    }
    
    public async Task<string> ExecutePromptAsync(string prompt)
    {
        // Create a function using the prompt
        var function = _kernel.CreateFunctionFromPrompt(prompt);
        
        // Execute the function
        var result = await _kernel.InvokeAsync(function);
        
        return result.GetValue<string>();
    }
    
    public async Task<string> GetChatResponseAsync(string userMessage, string systemPrompt = "You are a helpful assistant.")
    {
        // Create chat history
        var chatHistory = new ChatHistory(systemPrompt);
        
        // Add user message
        chatHistory.AddUserMessage(userMessage);
        
        // Get chat completion service
        var chatCompletionService = _kernel.GetRequiredService<IChatCompletionService>();
        
        // Get response
        var result = await chatCompletionService.GetChatMessageContentAsync(chatHistory);
        
        return result.Content;
    }
}
```

### 2. Vector Database Clients

#### Pinecone.NET

A .NET client for the Pinecone vector database service.

**Installation:**
```bash
dotnet add package Pinecone.NET
```

**Key Features:**
- Full API coverage for Pinecone vector database
- Async operations
- Strongly typed models
- Batch operations support

**Code Example:**
```csharp
using Pinecone;
using Pinecone.Grpc;

public class PineconeService
{
    private readonly PineconeClient _client;
    private readonly string _indexName;
    
    public PineconeService(string apiKey, string environment, string indexName)
    {
        var connectionConfig = new PineconeConnectionConfig
        {
            ApiKey = apiKey,
            Environment = environment
        };
        
        _client = new PineconeClient(connectionConfig);
        _indexName = indexName;
    }
    
    public async Task UpsertVectorsAsync(List<Vector> vectors)
    {
        var index = _client.Index(_indexName);
        await index.UpsertAsync(vectors);
    }
    
    public async Task<QueryResponse> QueryAsync(float[] queryVector, int topK = 5)
    {
        var index = _client.Index(_indexName);
        
        var request = new QueryRequest
        {
            Vector = queryVector,
            TopK = topK,
            IncludeMetadata = true
        };
        
        return await index.QueryAsync(request);
    }
}
```

#### Qdrant.NET

A .NET client for the Qdrant vector database.

**Installation:**
```bash
dotnet add package Qdrant.Client
```

**Key Features:**
- Complete API for Qdrant vector database
- Support for all Qdrant operations
- Strongly typed models
- Filtering capabilities

**Code Example:**
```csharp
using Qdrant.Client;
using Qdrant.Client.Grpc;

public class QdrantService
{
    private readonly QdrantClient _client;
    private readonly string _collectionName;
    
    public QdrantService(string host, int port, string collectionName)
    {
        _client = new QdrantClient(host, port);
        _collectionName = collectionName;
    }
    
    public async Task CreateCollectionAsync(int vectorSize)
    {
        var vectorParams = new VectorParams
        {
            Size = vectorSize,
            Distance = Distance.Cosine
        };
        
        await _client.CreateCollectionAsync(_collectionName, vectorParams);
    }
    
    public async Task UpsertPointsAsync(List<PointStruct> points)
    {
        await _client.UpsertAsync(_collectionName, points);
    }
    
    public async Task<List<ScoredPoint>> SearchAsync(float[] queryVector, int limit = 5)
    {
        var searchResult = await _client.SearchAsync(_collectionName, queryVector, limit: limit);
        return searchResult.ToList();
    }
}
```

### 3. Web Framework and API Libraries

#### ASP.NET Core

The primary web framework for building web applications and APIs with .NET.

**Installation:**
```bash
dotnet new webapi -n YourChatbotApi
```

**Key Features:**
- High-performance, cross-platform web framework
- Built-in dependency injection
- Middleware pipeline for request processing
- Comprehensive authentication and authorization

**Code Example - Chatbot API Controller:**
```csharp
using Microsoft.AspNetCore.Mvc;
using System.Threading.Tasks;

[ApiController]
[Route("api/[controller]")]
public class ChatbotController : ControllerBase
{
    private readonly IChatbotService _chatbotService;
    
    public ChatbotController(IChatbotService chatbotService)
    {
        _chatbotService = chatbotService;
    }
    
    [HttpPost("message")]
    public async Task<IActionResult> SendMessage([FromBody] ChatMessageRequest request)
    {
        if (string.IsNullOrEmpty(request.Message))
        {
            return BadRequest("Message cannot be empty");
        }
        
        var response = await _chatbotService.ProcessMessageAsync(
            request.Message, 
            request.ConversationId
        );
        
        return Ok(new ChatMessageResponse
        {
            Message = response,
            ConversationId = request.ConversationId
        });
    }
    
    [HttpGet("conversation/{conversationId}")]
    public async Task<IActionResult> GetConversation(string conversationId)
    {
        var conversation = await _chatbotService.GetConversationHistoryAsync(conversationId);
        
        if (conversation == null)
        {
            return NotFound();
        }
        
        return Ok(conversation);
    }
}

public class ChatMessageRequest
{
    public string Message { get; set; }
    public string ConversationId { get; set; }
}

public class ChatMessageResponse
{
    public string Message { get; set; }
    public string ConversationId { get; set; }
}
```

#### SignalR

A library for adding real-time web functionality to applications.

**Installation:**
```bash
dotnet add package Microsoft.AspNetCore.SignalR
```

**Key Features:**
- Real-time bidirectional communication
- Automatic reconnection
- Connection management
- Multiple transport methods (WebSockets, Server-Sent Events, Long Polling)

**Code Example - Chatbot Hub:**
```csharp
using Microsoft.AspNetCore.SignalR;
using System.Threading.Tasks;

public class ChatbotHub : Hub
{
    private readonly IChatbotService _chatbotService;
    
    public ChatbotHub(IChatbotService chatbotService)
    {
        _chatbotService = chatbotService;
    }
    
    public async Task SendMessage(string message, string conversationId)
    {
        // Process the message
        var response = await _chatbotService.ProcessMessageAsync(message, conversationId);
        
        // Send the response back to the caller
        await Clients.Caller.SendAsync("ReceiveMessage", response, conversationId);
    }
    
    public async Task JoinConversation(string conversationId)
    {
        await Groups.AddToGroupAsync(Context.ConnectionId, conversationId);
        
        // Get conversation history
        var history = await _chatbotService.GetConversationHistoryAsync(conversationId);
        
        // Send history to the caller
        await Clients.Caller.SendAsync("ReceiveHistory", history);
    }
}
```

### 4. Data Access and Storage

#### Entity Framework Core

An object-relational mapper (ORM) for .NET.

**Installation:**
```bash
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

**Key Features:**
- Object-relational mapping
- LINQ support for queries
- Change tracking
- Migrations for database schema updates

**Code Example - Conversation Storage:**
```csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

public class ChatbotDbContext : DbContext
{
    public ChatbotDbContext(DbContextOptions<ChatbotDbContext> options)
        : base(options)
    {
    }
    
    public DbSet<Conversation> Conversations { get; set; }
    public DbSet<Message> Messages { get; set; }
    
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Conversation>()
            .HasMany(c => c.Messages)
            .WithOne(m => m.Conversation)
            .HasForeignKey(m => m.ConversationId);
    }
}

public class Conversation
{
    public string Id { get; set; } = Guid.NewGuid().ToString();
    public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
    public DateTime LastActivityAt { get; set; } = DateTime.UtcNow;
    public List<Message> Messages { get; set; } = new List<Message>();
}

public class Message
{
    public string Id { get; set; } = Guid.NewGuid().ToString();
    public string ConversationId { get; set; }
    public string Content { get; set; }
    public string Role { get; set; } // "user" or "assistant"
    public DateTime Timestamp { get; set; } = DateTime.UtcNow;
    
    public Conversation Conversation { get; set; }
}

public class ConversationService
{
    private readonly ChatbotDbContext _dbContext;
    
    public ConversationService(ChatbotDbContext dbContext)
    {
        _dbContext = dbContext;
    }
    
    public async Task<Conversation> GetOrCreateConversationAsync(string conversationId)
    {
        var conversation = await _dbContext.Conversations
            .Include(c => c.Messages)
            .FirstOrDefaultAsync(c => c.Id == conversationId);
            
        if (conversation == null)
        {
            conversation = new Conversation { Id = conversationId };
            _dbContext.Conversations.Add(conversation);
            await _dbContext.SaveChangesAsync();
        }
        
        return conversation;
    }
    
    public async Task AddMessageAsync(string conversationId, string content, string role)
    {
        var conversation = await GetOrCreateConversationAsync(conversationId);
        
        var message = new Message
        {
            ConversationId = conversationId,
            Content = content,
            Role = role
        };
        
        conversation.Messages.Add(message);
        conversation.LastActivityAt = DateTime.UtcNow;
        
        await _dbContext.SaveChangesAsync();
    }
    
    public async Task<List<Message>> GetConversationHistoryAsync(string conversationId)
    {
        var conversation = await _dbContext.Conversations
            .Include(c => c.Messages)
            .FirstOrDefaultAsync(c => c.Id == conversationId);
            
        return conversation?.Messages.OrderBy(m => m.Timestamp).ToList() ?? new List<Message>();
    }
}
```

#### MongoDB.Driver

The official .NET driver for MongoDB.

**Installation:**
```bash
dotnet add package MongoDB.Driver
```

**Key Features:**
- Full MongoDB API support
- LINQ provider
- Strongly typed document mapping
- Connection pooling

**Code Example - Document Storage:**
```csharp
using MongoDB.Bson;
using MongoDB.Driver;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

public class MongoDbService
{
    private readonly IMongoDatabase _database;
    
    public MongoDbService(string connectionString, string databaseName)
    {
        var client = new MongoClient(connectionString);
        _database = client.GetDatabase(databaseName);
    }
    
    public IMongoCollection<T> GetCollection<T>(string collectionName)
    {
        return _database.GetCollection<T>(collectionName);
    }
}

public class Conversation
{
    public ObjectId Id { get; set; }
    public string ConversationId { get; set; }
    public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
    public DateTime LastActivityAt { get; set; } = DateTime.UtcNow;
    public List<Message> Messages { get; set; } = new List<Message>();
}

public class Message
{
    public string Content { get; set; }
    public string Role { get; set; } // "user" or "assistant"
    public DateTime Timestamp { get; set; } = DateTime.UtcNow;
}

public class ConversationRepository
{
    private readonly IMongoCollection<Conversation> _conversations;
    
    public ConversationRepository(MongoDbService mongoDbService)
    {
        _conversations = mongoDbService.GetCollection<Conversation>("conversations");
    }
    
    public async Task<Conversation> GetOrCreateConversationAsync(string conversationId)
    {
        var conversation = await _conversations
            .Find(c => c.ConversationId == conversationId)
            .FirstOrDefaultAsync();
            
        if (conversation == null)
        {
            conversation = new Conversation { ConversationId = conversationId };
            await _conversations.InsertOneAsync(conversation);
        }
        
        return conversation;
    }
    
    public async Task AddMessageAsync(string conversationId, string content, string role)
    {
        var message = new Message
        {
            Content = content,
            Role = role
        };
        
        var update = Builders<Conversation>.Update
            .Push(c => c.Messages, message)
            .Set(c => c.LastActivityAt, DateTime.UtcNow);
            
        await _conversations.UpdateOneAsync(
            c => c.ConversationId == conversationId,
            update
        );
    }
    
    public async Task<List<Message>> GetConversationHistoryAsync(string conversationId)
    {
        var conversation = await _conversations
            .Find(c => c.ConversationId == conversationId)
            .FirstOrDefaultAsync();
            
        return conversation?.Messages ?? new List<Message>();
    }
}
```

### 5. Email Integration Libraries

#### MailKit

A cross-platform .NET library for IMAP, POP3, and SMTP.

**Installation:**
```bash
dotnet add package MailKit
```

**Key Features:**
- Complete email protocol support
- MIME parsing and creation
- Secure authentication
- Event-driven email monitoring

**Code Example - Email Processing:**
```csharp
using MailKit;
using MailKit.Net.Imap;
using MailKit.Search;
using MimeKit;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

public class EmailService
{
    private readonly string _host;
    private readonly int _port;
    private readonly string _username;
    private readonly string _password;
    
    public EmailService(string host, int port, string username, string password)
    {
        _host = host;
        _port = port;
        _username = username;
        _password = password;
    }
    
    public async Task<List<MimeMessage>> GetUnreadEmailsAsync()
    {
        using var client = new ImapClient();
        
        await client.ConnectAsync(_host, _port, true);
        await client.AuthenticateAsync(_username, _password);
        
        var inbox = client.Inbox;
        await inbox.OpenAsync(FolderAccess.ReadWrite);
        
        var uids = await inbox.SearchAsync(SearchQuery.NotSeen);
        var messages = new List<MimeMessage>();
        
        foreach (var uid in uids)
        {
            var message = await inbox.GetMessageAsync(uid);
            messages.Add(message);
            
            // Mark as seen
            await inbox.AddFlagsAsync(uid, MessageFlags.Seen, true);
        }
        
        await client.DisconnectAsync(true);
        
        return messages;
    }
    
    public async Task SendEmailAsync(string to, string subject, string body, bool isHtml = false)
    {
        var message = new MimeMessage();
        message.From.Add(new MailboxAddress("Chatbot", _username));
        message.To.Add(new MailboxAddress("", to));
        message.Subject = subject;
        
        var bodyPart = new TextPart(isHtml ? "html" : "plain")
        {
            Text = body
        };
        
        message.Body = bodyPart;
        
        using var client = new MailKit.Net.Smtp.SmtpClient();
        await client.ConnectAsync(_host, _port, true);
        await client.AuthenticateAsync(_username, _password);
        await client.SendAsync(message);
        await client.DisconnectAsync(true);
    }
}
```

#### Microsoft.Graph

The official .NET SDK for Microsoft Graph API, useful for integrating with Microsoft 365 services.

**Installation:**
```bash
dotnet add package Microsoft.Graph
```

**Key Features:**
- Access to Microsoft 365 services (Outlook, Teams, etc.)
- Authentication with Microsoft identity platform
- Comprehensive API coverage
- Batch requests

**Code Example - Email Processing with Microsoft Graph:**
```csharp
using Microsoft.Graph;
using Microsoft.Identity.Client;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

public class GraphEmailService
{
    private readonly GraphServiceClient _graphClient;
    
    public GraphEmailService(string tenantId, string clientId, string clientSecret)
    {
        // Configure authentication
        var confidentialClientApp = ConfidentialClientApplicationBuilder
            .Create(clientId)
            .WithTenantId(tenantId)
            .WithClientSecret(clientSecret)
            .Build();
            
        var scopes = new[] { "https://graph.microsoft.com/.default" };
        
        // Create Graph client
        _graphClient = new GraphServiceClient(
            new DelegateAuthenticationProvider(async (requestMessage) =>
            {
                var authResult = await confidentialClientApp
                    .AcquireTokenForClient(scopes)
                    .ExecuteAsync();
                    
                requestMessage.Headers.Authorization = 
                    new System.Net.Http.Headers.AuthenticationHeaderValue(
                        "Bearer", authResult.AccessToken);
            })
        );
    }
    
    public async Task<List<Message>> GetUnreadEmailsAsync(string userEmail)
    {
        var messages = await _graphClient.Users[userEmail].Messages
            .Request()
            .Filter("isRead eq false")
            .Top(50)
            .GetAsync();
            
        var result = new List<Message>();
        var pageIterator = PageIterator<Message>
            .CreatePageIterator(_graphClient, messages, (m) =>
            {
                result.Add(m);
                return true;
            });
            
        await pageIterator.IterateAsync();
        
        return result;
    }
    
    public async Task SendEmailAsync(string fromEmail, string toEmail, string subject, string body, bool isHtml = false)
    {
        var message = new Message
        {
            Subject = subject,
            Body = new ItemBody
            {
                ContentType = isHtml ? BodyType.Html : BodyType.Text,
                Content = body
            },
            ToRecipients = new List<Recipient>
            {
                new Recipient
                {
                    EmailAddress = new EmailAddress
                    {
                        Address = toEmail
                    }
                }
            }
        };
        
        await _graphClient.Users[fromEmail].SendMail(message, true).Request().PostAsync();
    }
    
    public async Task MarkEmailAsReadAsync(string userEmail, string messageId)
    {
        await _graphClient.Users[userEmail].Messages[messageId]
            .Request()
            .UpdateAsync(new Message { IsRead = true });
    }
}
```

### 6. Database Connectivity Libraries

#### Dapper

A high-performance micro-ORM for .NET.

**Installation:**
```bash
dotnet add package Dapper
```

**Key Features:**
- Lightweight and fast
- Simple SQL execution
- Object mapping
- Multiple result sets

**Code Example - Data Access:**
```csharp
using Dapper;
using Microsoft.Data.SqlClient;
using System.Collections.Generic;
using System.Data;
using System.Threading.Tasks;

public class DataRepository
{
    private readonly string _connectionString;
    
    public DataRepository(string connectionString)
    {
        _connectionString = connectionString;
    }
    
    public async Task<IEnumerable<T>> QueryAsync<T>(string sql, object parameters = null)
    {
        using var connection = new SqlConnection(_connectionString);
        await connection.OpenAsync();
        
        return await connection.QueryAsync<T>(sql, parameters);
    }
    
    public async Task<T> QuerySingleOrDefaultAsync<T>(string sql, object parameters = null)
    {
        using var connection = new SqlConnection(_connectionString);
        await connection.OpenAsync();
        
        return await connection.QuerySingleOrDefaultAsync<T>(sql, parameters);
    }
    
    public async Task<int> ExecuteAsync(string sql, object parameters = null)
    {
        using var connection = new SqlConnection(_connectionString);
        await connection.OpenAsync();
        
        return await connection.ExecuteAsync(sql, parameters);
    }
}

// Example usage for real-time data Q&A
public class SalesDataService
{
    private readonly DataRepository _repository;
    
    public SalesDataService(DataRepository repository)
    {
        _repository = repository;
    }
    
    public async Task<decimal> GetTotalSalesByRegionAsync(string region, DateTime startDate, DateTime endDate)
    {
        var sql = @"
            SELECT SUM(Amount) AS TotalSales
            FROM Sales
            WHERE Region = @Region
            AND SaleDate BETWEEN @StartDate AND @EndDate";
            
        var parameters = new
        {
            Region = region,
            StartDate = startDate,
            EndDate = endDate
        };
        
        return await _repository.QuerySingleOrDefaultAsync<decimal>(sql, parameters);
    }
    
    public async Task<IEnumerable<ProductSales>> GetTopSellingProductsAsync(int count)
    {
        var sql = @"
            SELECT TOP(@Count) ProductName, SUM(Quantity) AS TotalQuantity, SUM(Amount) AS TotalAmount
            FROM Sales
            GROUP BY ProductName
            ORDER BY SUM(Amount) DESC";
            
        return await _repository.QueryAsync<ProductSales>(sql, new { Count = count });
    }
}

public class ProductSales
{
    public string ProductName { get; set; }
    public int TotalQuantity { get; set; }
    public decimal TotalAmount { get; set; }
}
```

## JavaScript Libraries and Frameworks

### 1. LLM Integration Libraries

#### OpenAI Node.js Library

The official Node.js library for the OpenAI API.

**Installation:**
```bash
npm install openai
```

**Key Features:**
- Complete API coverage
- TypeScript support
- Streaming responses
- Automatic retries

**Code Example - Text Generation:**
```javascript
const { OpenAI } = require('openai');

class OpenAiService {
  constructor(apiKey) {
    this.openai = new OpenAI({ apiKey });
  }
  
  async generateText(prompt) {
    const response = await this.openai.completions.create({
      model: 'gpt-3.5-turbo-instruct',
      prompt,
      max_tokens: 500,
      temperature: 0.7
    });
    
    return response.choices[0].text;
  }
  
  async getChatResponse(messages) {
    const response = await this.openai.chat.completions.create({
      model: 'gpt-4',
      messages,
      temperature: 0.7,
      max_tokens: 500
    });
    
    return response.choices[0].message.content;
  }
  
  async generateEmbedding(text) {
    const response = await this.openai.embeddings.create({
      model: 'text-embedding-ada-002',
      input: text
    });
    
    return response.data[0].embedding;
  }
}

module.exports = OpenAiService;
```

#### Anthropic SDK for JavaScript

The official JavaScript SDK for Anthropic's Claude models.

**Installation:**
```bash
npm install @anthropic-ai/sdk
```

**Key Features:**
- Access to Claude models
- Streaming support
- TypeScript definitions
- Error handling

**Code Example:**
```javascript
const Anthropic = require('@anthropic-ai/sdk');

class AnthropicService {
  constructor(apiKey) {
    this.anthropic = new Anthropic({ apiKey });
  }
  
  async getChatResponse(systemPrompt, userMessage) {
    const response = await this.anthropic.messages.create({
      model: 'claude-2',
      system: systemPrompt,
      messages: [
        { role: 'user', content: userMessage }
      ],
      max_tokens: 1000
    });
    
    return response.content[0].text;
  }
  
  async streamChatResponse(systemPrompt, userMessage, onChunk) {
    const stream = await this.anthropic.messages.create({
      model: 'claude-2',
      system: systemPrompt,
      messages: [
        { role: 'user', content: userMessage }
      ],
      max_tokens: 1000,
      stream: true
    });
    
    let fullResponse = '';
    
    for await (const chunk of stream) {
      if (chunk.type === 'content_block_delta' && chunk.delta.text) {
        fullResponse += chunk.delta.text;
        onChunk(chunk.delta.text);
      }
    }
    
    return fullResponse;
  }
}

module.exports = AnthropicService;
```

#### LangChain.js

A framework for building applications with LLMs.

**Installation:**
```bash
npm install langchain
```

**Key Features:**
- Chains for combining LLM calls with other components
- Document loaders and text splitters
- Vector stores integration
- Agents for complex reasoning

**Code Example - RAG Implementation:**
```javascript
const { OpenAI } = require('langchain/llms/openai');
const { RetrievalQAChain } = require('langchain/chains');
const { HNSWLib } = require('langchain/vectorstores/hnswlib');
const { OpenAIEmbeddings } = require('langchain/embeddings/openai');
const { RecursiveCharacterTextSplitter } = require('langchain/text_splitter');
const { CheerioWebBaseLoader } = require('langchain/document_loaders/web/cheerio');

class RagService {
  constructor(openAiApiKey) {
    this.embeddings = new OpenAIEmbeddings({ openAIApiKey });
    this.model = new OpenAI({ 
      openAIApiKey, 
      modelName: 'gpt-4',
      temperature: 0.7
    });
  }
  
  async loadAndIndexWebsite(url) {
    // Load content from website
    const loader = new CheerioWebBaseLoader(url);
    const docs = await loader.load();
    
    // Split text into chunks
    const textSplitter = new RecursiveCharacterTextSplitter({
      chunkSize: 1000,
      chunkOverlap: 200
    });
    const splitDocs = await textSplitter.splitDocuments(docs);
    
    // Create vector store
    this.vectorStore = await HNSWLib.fromDocuments(splitDocs, this.embeddings);
    
    return splitDocs.length;
  }
  
  async answerQuestion(question) {
    if (!this.vectorStore) {
      throw new Error('Vector store not initialized. Call loadAndIndexWebsite first.');
    }
    
    // Create retrieval chain
    const chain = RetrievalQAChain.fromLLM(
      this.model,
      this.vectorStore.asRetriever()
    );
    
    // Get answer
    const response = await chain.call({ query: question });
    
    return response.text;
  }
  
  async saveVectorStore(directory) {
    if (!this.vectorStore) {
      throw new Error('Vector store not initialized');
    }
    
    await this.vectorStore.save(directory);
  }
  
  async loadVectorStore(directory) {
    this.vectorStore = await HNSWLib.load(directory, this.embeddings);
  }
}

module.exports = RagService;
```

### 2. Vector Database Clients

#### Pinecone Client for JavaScript

The official JavaScript client for Pinecone vector database.

**Installation:**
```bash
npm install @pinecone-database/pinecone
```

**Key Features:**
- Complete API coverage
- TypeScript support
- Batch operations
- Connection pooling

**Code Example:**
```javascript
const { Pinecone } = require('@pinecone-database/pinecone');

class PineconeService {
  constructor(apiKey, environment) {
    this.pinecone = new Pinecone({
      apiKey,
      environment
    });
  }
  
  async createIndex(indexName, dimension) {
    await this.pinecone.createIndex({
      name: indexName,
      dimension,
      metric: 'cosine'
    });
    
    // Wait for index to be ready
    await new Promise(resolve => setTimeout(resolve, 60000));
    
    return this.pinecone.index(indexName);
  }
  
  getIndex(indexName) {
    return this.pinecone.index(indexName);
  }
  
  async upsertVectors(index, vectors) {
    // vectors should be an array of objects with id, values, and metadata
    await index.upsert(vectors);
  }
  
  async queryVectors(index, queryVector, topK = 5, filter = {}) {
    const queryResponse = await index.query({
      vector: queryVector,
      topK,
      includeMetadata: true,
      filter
    });
    
    return queryResponse.matches;
  }
  
  async deleteVectors(index, ids) {
    await index.deleteMany(ids);
  }
}

module.exports = PineconeService;
```

#### Weaviate JavaScript Client

The official JavaScript client for Weaviate vector database.

**Installation:**
```bash
npm install weaviate-ts-client
```

**Key Features:**
- GraphQL-like query interface
- Schema management
- Batch operations
- Real-time capabilities

**Code Example:**
```javascript
const weaviate = require('weaviate-ts-client');

class WeaviateService {
  constructor(host, apiKey = null) {
    const clientConfig = {
      scheme: 'https',
      host
    };
    
    if (apiKey) {
      clientConfig.apiKey = new weaviate.ApiKey(apiKey);
    }
    
    this.client = weaviate.client(clientConfig);
  }
  
  async createSchema(className, properties) {
    const classObj = {
      class: className,
      vectorizer: 'text2vec-openai',
      properties
    };
    
    await this.client.schema.classCreator().withClass(classObj).do();
  }
  
  async addData(className, objects) {
    // Batch import objects
    const batcher = this.client.batch.objectsBatcher();
    
    for (const obj of objects) {
      const weaviateObj = {
        class: className,
        properties: obj.properties
      };
      
      batcher.withObject(weaviateObj);
    }
    
    return await batcher.do();
  }
  
  async semanticSearch(className, query, properties, limit = 5) {
    const result = await this.client.graphql
      .get()
      .withClassName(className)
      .withFields(properties.join(' '))
      .withNearText({ concepts: [query] })
      .withLimit(limit)
      .do();
      
    return result.data.Get[className];
  }
  
  async deleteClass(className) {
    await this.client.schema.classDeleter().withClassName(className).do();
  }
}

module.exports = WeaviateService;
```

### 3. Web Framework and API Libraries

#### Express.js

A minimal and flexible Node.js web application framework.

**Installation:**
```bash
npm install express
```

**Key Features:**
- Routing
- Middleware support
- HTTP utility methods
- Template engine support

**Code Example - Chatbot API:**
```javascript
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const { v4: uuidv4 } = require('uuid');

class ChatbotApi {
  constructor(chatbotService) {
    this.chatbotService = chatbotService;
    this.app = express();
    
    // Middleware
    this.app.use(cors());
    this.app.use(bodyParser.json());
    
    // Routes
    this.setupRoutes();
  }
  
  setupRoutes() {
    // Health check
    this.app.get('/health', (req, res) => {
      res.status(200).json({ status: 'ok' });
    });
    
    // Chat endpoint
    this.app.post('/api/chat', async (req, res) => {
      try {
        const { message, conversationId = uuidv4() } = req.body;
        
        if (!message) {
          return res.status(400).json({ error: 'Message is required' });
        }
        
        const response = await this.chatbotService.processMessage(message, conversationId);
        
        res.status(200).json({
          response,
          conversationId
        });
      } catch (error) {
        console.error('Error processing message:', error);
        res.status(500).json({ error: 'Failed to process message' });
      }
    });
    
    // Get conversation history
    this.app.get('/api/conversations/:conversationId', async (req, res) => {
      try {
        const { conversationId } = req.params;
        const history = await this.chatbotService.getConversationHistory(conversationId);
        
        res.status(200).json({ history });
      } catch (error) {
        console.error('Error fetching conversation:', error);
        res.status(500).json({ error: 'Failed to fetch conversation' });
      }
    });
  }
  
  start(port = 3000) {
    this.server = this.app.listen(port, () => {
      console.log(`Chatbot API running on port ${port}`);
    });
    
    return this.server;
  }
  
  stop() {
    if (this.server) {
      this.server.close();
    }
  }
}

module.exports = ChatbotApi;
```

#### Next.js

A React framework for building full-stack web applications.

**Installation:**
```bash
npx create-next-app@latest my-chatbot-app
```

**Key Features:**
- Server-side rendering
- API routes
- File-based routing
- Built-in optimization

**Code Example - Chatbot Frontend and API:**
```javascript
// pages/api/chat.js - API Route
import { OpenAI } from 'openai';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY
});

export default async function handler(req, res) {
  if (req.method !== 'POST') {
    return res.status(405).json({ error: 'Method not allowed' });
  }
  
  try {
    const { message, conversationId } = req.body;
    
    if (!message) {
      return res.status(400).json({ error: 'Message is required' });
    }
    
    // Process with OpenAI
    const response = await openai.chat.completions.create({
      model: 'gpt-4',
      messages: [
        { role: 'system', content: 'You are a helpful assistant.' },
        { role: 'user', content: message }
      ]
    });
    
    const reply = response.choices[0].message.content;
    
    // In a real app, you would save the conversation to a database here
    
    res.status(200).json({
      response: reply,
      conversationId: conversationId || 'new-conversation'
    });
  } catch (error) {
    console.error('Error processing chat:', error);
    res.status(500).json({ error: 'Failed to process chat' });
  }
}

// pages/index.js - Frontend
import { useState, useEffect, useRef } from 'react';
import styles from '../styles/Home.module.css';

export default function Home() {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(false);
  const [conversationId, setConversationId] = useState(null);
  const messagesEndRef = useRef(null);
  
  // Auto-scroll to bottom when messages change
  useEffect(() => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages]);
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    if (!input.trim()) return;
    
    // Add user message to chat
    const userMessage = { text: input, sender: 'user' };
    setMessages(prev => [...prev, userMessage]);
    setInput('');
    setLoading(true);
    
    try {
      // Call API
      const response = await fetch('/api/chat', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ 
          message: input,
          conversationId
        })
      });
      
      const data = await response.json();
      
      // Add bot response to chat
      setMessages(prev => [...prev, { text: data.response, sender: 'bot' }]);
      
      // Save conversation ID if new
      if (!conversationId) {
        setConversationId(data.conversationId);
      }
    } catch (error) {
      console.error('Error:', error);
      setMessages(prev => [...prev, { 
        text: 'Sorry, I encountered an error. Please try again.',
        sender: 'bot',
        isError: true
      }]);
    } finally {
      setLoading(false);
    }
  };
  
  return (
    <div className={styles.container}>
      <main className={styles.main}>
        <h1 className={styles.title}>AI Chatbot</h1>
        
        <div className={styles.chatContainer}>
          <div className={styles.messages}>
            {messages.length === 0 && (
              <div className={styles.welcome}>
                How can I help you today?
              </div>
            )}
            
            {messages.map((msg, index) => (
              <div key={index} className={`${styles.message} ${styles[msg.sender]}`}>
                {msg.text}
              </div>
            ))}
            
            {loading && (
              <div className={`${styles.message} ${styles.bot} ${styles.loading}`}>
                <span className={styles.typingIndicator}></span>
              </div>
            )}
            
            <div ref={messagesEndRef} />
          </div>
          
          <form className={styles.inputForm} onSubmit={handleSubmit}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Type your message..."
              disabled={loading}
              className={styles.input}
            />
            <button 
              type="submit" 
              disabled={loading || !input.trim()}
              className={styles.sendButton}
            >
              Send
            </button>
          </form>
        </div>
      </main>
    </div>
  );
}
```

#### Socket.IO

A library for real-time web applications.

**Installation:**
```bash
npm install socket.io
```

**Key Features:**
- Real-time bidirectional communication
- Automatic reconnection
- Room and namespace support
- Fallback to long polling if WebSockets unavailable

**Code Example - Real-time Chat:**
```javascript
// Server-side (with Express)
const express = require('express');
const http = require('http');
const { Server } = require('socket.io');
const ChatbotService = require('./services/ChatbotService');

class ChatServer {
  constructor(chatbotService) {
    this.chatbotService = chatbotService;
    this.app = express();
    this.server = http.createServer(this.app);
    this.io = new Server(this.server, {
      cors: {
        origin: '*',
        methods: ['GET', 'POST']
      }
    });
    
    this.setupSocketHandlers();
  }
  
  setupSocketHandlers() {
    this.io.on('connection', (socket) => {
      console.log('User connected:', socket.id);
      
      // Handle chat messages
      socket.on('sendMessage', async (data) => {
        try {
          const { message, conversationId } = data;
          
          // Process message with chatbot service
          const response = await this.chatbotService.processMessage(message, conversationId);
          
          // Send response back to the client
          socket.emit('receiveMessage', {
            response,
            conversationId
          });
        } catch (error) {
          console.error('Error processing message:', error);
          socket.emit('error', { message: 'Failed to process message' });
        }
      });
      
      // Join a specific conversation
      socket.on('joinConversation', (conversationId) => {
        socket.join(conversationId);
        console.log(`User ${socket.id} joined conversation ${conversationId}`);
      });
      
      // Handle disconnection
      socket.on('disconnect', () => {
        console.log('User disconnected:', socket.id);
      });
    });
  }
  
  start(port = 3000) {
    this.server.listen(port, () => {
      console.log(`Chat server running on port ${port}`);
    });
  }
}

// Usage
const chatbotService = new ChatbotService();
const chatServer = new ChatServer(chatbotService);
chatServer.start(3000);

// Client-side
import { useEffect, useState } from 'react';
import { io } from 'socket.io-client';

function ChatComponent() {
  const [socket, setSocket] = useState(null);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [conversationId, setConversationId] = useState('default-conversation');
  
  useEffect(() => {
    // Initialize socket connection
    const newSocket = io('http://localhost:3000');
    setSocket(newSocket);
    
    // Join conversation
    newSocket.emit('joinConversation', conversationId);
    
    // Set up event listeners
    newSocket.on('receiveMessage', (data) => {
      setMessages(prev => [...prev, { text: data.response, sender: 'bot' }]);
    });
    
    newSocket.on('error', (error) => {
      console.error('Socket error:', error);
      setMessages(prev => [...prev, { 
        text: 'Sorry, an error occurred. Please try again.',
        sender: 'bot',
        isError: true
      }]);
    });
    
    // Clean up on unmount
    return () => {
      newSocket.disconnect();
    };
  }, [conversationId]);
  
  const sendMessage = (e) => {
    e.preventDefault();
    
    if (!input.trim() || !socket) return;
    
    // Add user message to chat
    setMessages(prev => [...prev, { text: input, sender: 'user' }]);
    
    // Send message to server
    socket.emit('sendMessage', {
      message: input,
      conversationId
    });
    
    setInput('');
  };
  
  return (
    <div className="chat-container">
      <div className="messages">
        {messages.map((msg, index) => (
          <div key={index} className={`message ${msg.sender}`}>
            {msg.text}
          </div>
        ))}
      </div>
      
      <form onSubmit={sendMessage}>
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="Type your message..."
        />
        <button type="submit">Send</button>
      </form>
    </div>
  );
}

export default ChatComponent;
```

### 4. Data Access and Storage

#### MongoDB Node.js Driver

The official MongoDB driver for Node.js.

**Installation:**
```bash
npm install mongodb
```

**Key Features:**
- Full MongoDB API support
- Connection pooling
- CRUD operations
- Aggregation framework

**Code Example - Conversation Storage:**
```javascript
const { MongoClient, ObjectId } = require('mongodb');

class ConversationRepository {
  constructor(connectionString, dbName) {
    this.connectionString = connectionString;
    this.dbName = dbName;
    this.client = null;
    this.db = null;
  }
  
  async connect() {
    if (!this.client) {
      this.client = new MongoClient(this.connectionString);
      await this.client.connect();
      this.db = this.client.db(this.dbName);
      console.log('Connected to MongoDB');
    }
    return this.db;
  }
  
  async getCollection(collectionName) {
    const db = await this.connect();
    return db.collection(collectionName);
  }
  
  async saveMessage(conversationId, message, role) {
    const collection = await this.getCollection('conversations');
    
    // Check if conversation exists
    const conversation = await collection.findOne({ conversationId });
    
    if (conversation) {
      // Add message to existing conversation
      await collection.updateOne(
        { conversationId },
        { 
          $push: { 
            messages: { 
              content: message, 
              role, 
              timestamp: new Date() 
            } 
          },
          $set: { lastUpdated: new Date() }
        }
      );
    } else {
      // Create new conversation
      await collection.insertOne({
        conversationId,
        createdAt: new Date(),
        lastUpdated: new Date(),
        messages: [
          { content: message, role, timestamp: new Date() }
        ]
      });
    }
    
    return conversationId;
  }
  
  async getConversation(conversationId) {
    const collection = await this.getCollection('conversations');
    return collection.findOne({ conversationId });
  }
  
  async listConversations(limit = 10, skip = 0) {
    const collection = await this.getCollection('conversations');
    
    return collection
      .find({})
      .sort({ lastUpdated: -1 })
      .skip(skip)
      .limit(limit)
      .project({ conversationId: 1, createdAt: 1, lastUpdated: 1 })
      .toArray();
  }
  
  async deleteConversation(conversationId) {
    const collection = await this.getCollection('conversations');
    const result = await collection.deleteOne({ conversationId });
    return result.deletedCount > 0;
  }
  
  async close() {
    if (this.client) {
      await this.client.close();
      this.client = null;
      this.db = null;
      console.log('Disconnected from MongoDB');
    }
  }
}

module.exports = ConversationRepository;
```

#### Mongoose

An elegant MongoDB object modeling for Node.js.

**Installation:**
```bash
npm install mongoose
```

**Key Features:**
- Schema-based modeling
- Validation
- Query building
- Middleware support

**Code Example - Conversation Model:**
```javascript
const mongoose = require('mongoose');

// Define schemas
const messageSchema = new mongoose.Schema({
  content: {
    type: String,
    required: true
  },
  role: {
    type: String,
    enum: ['user', 'assistant', 'system'],
    required: true
  },
  timestamp: {
    type: Date,
    default: Date.now
  }
});

const conversationSchema = new mongoose.Schema({
  conversationId: {
    type: String,
    required: true,
    unique: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  },
  lastUpdated: {
    type: Date,
    default: Date.now
  },
  messages: [messageSchema]
});

// Create model
const Conversation = mongoose.model('Conversation', conversationSchema);

class ConversationService {
  constructor(connectionString) {
    this.connectionString = connectionString;
    this.isConnected = false;
  }
  
  async connect() {
    if (!this.isConnected) {
      await mongoose.connect(this.connectionString);
      this.isConnected = true;
      console.log('Connected to MongoDB with Mongoose');
    }
  }
  
  async saveMessage(conversationId, content, role) {
    await this.connect();
    
    const message = { content, role, timestamp: new Date() };
    
    // Find and update, or create if not exists
    const conversation = await Conversation.findOneAndUpdate(
      { conversationId },
      { 
        $push: { messages: message },
        $set: { lastUpdated: new Date() },
        $setOnInsert: { createdAt: new Date() }
      },
      { 
        new: true, 
        upsert: true 
      }
    );
    
    return conversation;
  }
  
  async getConversation(conversationId) {
    await this.connect();
    return Conversation.findOne({ conversationId });
  }
  
  async getConversationHistory(conversationId) {
    await this.connect();
    const conversation = await Conversation.findOne({ conversationId });
    return conversation ? conversation.messages : [];
  }
  
  async listConversations(limit = 10, skip = 0) {
    await this.connect();
    
    return Conversation
      .find({})
      .sort({ lastUpdated: -1 })
      .skip(skip)
      .limit(limit)
      .select('conversationId createdAt lastUpdated');
  }
  
  async deleteConversation(conversationId) {
    await this.connect();
    const result = await Conversation.deleteOne({ conversationId });
    return result.deletedCount > 0;
  }
  
  async disconnect() {
    if (this.isConnected) {
      await mongoose.disconnect();
      this.isConnected = false;
      console.log('Disconnected from MongoDB');
    }
  }
}

module.exports = { Conversation, ConversationService };
```

### 5. Email Integration Libraries

#### Nodemailer

A module for Node.js applications to send emails.

**Installation:**
```bash
npm install nodemailer
```

**Key Features:**
- SMTP transport
- HTML email support
- Attachments
- Unicode support

**Code Example - Email Sending:**
```javascript
const nodemailer = require('nodemailer');

class EmailService {
  constructor(config) {
    this.transporter = nodemailer.createTransport({
      host: config.host,
      port: config.port,
      secure: config.secure || false,
      auth: {
        user: config.user,
        pass: config.password
      }
    });
  }
  
  async sendEmail(options) {
    const mailOptions = {
      from: options.from,
      to: options.to,
      subject: options.subject,
      text: options.text,
      html: options.html
    };
    
    if (options.attachments) {
      mailOptions.attachments = options.attachments;
    }
    
    try {
      const info = await this.transporter.sendMail(mailOptions);
      return {
        success: true,
        messageId: info.messageId
      };
    } catch (error) {
      console.error('Error sending email:', error);
      return {
        success: false,
        error: error.message
      };
    }
  }
  
  async verifyConnection() {
    try {
      await this.transporter.verify();
      return true;
    } catch (error) {
      console.error('Email connection verification failed:', error);
      return false;
    }
  }
}

// Usage example
async function main() {
  const emailService = new EmailService({
    host: 'smtp.example.com',
    port: 587,
    user: 'user@example.com',
    password: 'password'
  });
  
  const isConnected = await emailService.verifyConnection();
  
  if (isConnected) {
    const result = await emailService.sendEmail({
      from: '"Company Support" <support@example.com>',
      to: 'customer@example.com',
      subject: 'Your Support Ticket #12345',
      text: 'Thank you for contacting us. We have received your support request.',
      html: '<p>Thank you for contacting us. We have received your support request.</p>'
    });
    
    console.log('Email sent:', result);
  }
}

module.exports = EmailService;
```

#### Imap

A Node.js module to work with IMAP protocol.

**Installation:**
```bash
npm install imap mailparser
```

**Key Features:**
- IMAP protocol support
- Email fetching and searching
- Mailbox management
- Event-based API

**Code Example - Email Fetching:**
```javascript
const Imap = require('imap');
const { simpleParser } = require('mailparser');
const EventEmitter = require('events');

class EmailFetcherService extends EventEmitter {
  constructor(config) {
    super();
    
    this.config = {
      user: config.user,
      password: config.password,
      host: config.host,
      port: config.port || 993,
      tls: config.tls !== false,
      tlsOptions: config.tlsOptions || { rejectUnauthorized: true }
    };
    
    this.imap = new Imap(this.config);
    this.setupEventHandlers();
  }
  
  setupEventHandlers() {
    this.imap.on('ready', () => {
      this.emit('ready');
    });
    
    this.imap.on('error', (err) => {
      this.emit('error', err);
    });
    
    this.imap.on('end', () => {
      this.emit('end');
    });
  }
  
  connect() {
    return new Promise((resolve, reject) => {
      this.imap.once('ready', resolve);
      this.imap.once('error', reject);
      this.imap.connect();
    });
  }
  
  openInbox() {
    return new Promise((resolve, reject) => {
      this.imap.openBox('INBOX', false, (err, box) => {
        if (err) {
          reject(err);
        } else {
          resolve(box);
        }
      });
    });
  }
  
  async fetchUnreadEmails() {
    try {
      await this.connect();
      await this.openInbox();
      
      return new Promise((resolve, reject) => {
        this.imap.search(['UNSEEN'], (err, results) => {
          if (err) {
            reject(err);
            return;
          }
          
          if (!results || results.length === 0) {
            resolve([]);
            return;
          }
          
          const emails = [];
          
          const fetch = this.imap.fetch(results, { bodies: '' });
          
          fetch.on('message', (msg) => {
            msg.on('body', (stream) => {
              simpleParser(stream, async (err, parsed) => {
                if (err) {
                  console.error('Error parsing email:', err);
                } else {
                  emails.push({
                    id: parsed.messageId,
                    from: parsed.from.text,
                    to: parsed.to.text,
                    subject: parsed.subject,
                    text: parsed.text,
                    html: parsed.html,
                    date: parsed.date
                  });
                }
              });
            });
          });
          
          fetch.on('error', (err) => {
            reject(err);
          });
          
          fetch.on('end', () => {
            this.imap.end();
            resolve(emails);
          });
        });
      });
    } catch (error) {
      this.imap.end();
      throw error;
    }
  }
  
  async markEmailAsRead(uid) {
    try {
      await this.connect();
      await this.openInbox();
      
      return new Promise((resolve, reject) => {
        this.imap.setFlags([uid], ['\\Seen'], (err) => {
          this.imap.end();
          
          if (err) {
            reject(err);
          } else {
            resolve(true);
          }
        });
      });
    } catch (error) {
      this.imap.end();
      throw error;
    }
  }
}

module.exports = EmailFetcherService;
```

### 6. UI Component Libraries

#### React

A JavaScript library for building user interfaces.

**Installation:**
```bash
npx create-react-app my-chatbot-ui
```

**Key Features:**
- Component-based architecture
- Virtual DOM for performance
- Unidirectional data flow
- Rich ecosystem

**Code Example - Chatbot UI Component:**
```jsx
import React, { useState, useEffect, useRef } from 'react';
import './ChatbotUI.css';

const ChatbotUI = ({ onSendMessage, initialMessages = [] }) => {
  const [messages, setMessages] = useState(initialMessages);
  const [input, setInput] = useState('');
  const [isTyping, setIsTyping] = useState(false);
  const messagesEndRef = useRef(null);
  
  // Auto-scroll to bottom when messages change
  useEffect(() => {
    scrollToBottom();
  }, [messages]);
  
  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  };
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    if (!input.trim()) return;
    
    // Add user message
    const userMessage = { text: input, sender: 'user' };
    setMessages(prev => [...prev, userMessage]);
    setInput('');
    
    // Show typing indicator
    setIsTyping(true);
    
    try {
      // Get response from parent component
      const response = await onSendMessage(input);
      
      // Add bot response
      setMessages(prev => [...prev, { text: response, sender: 'bot' }]);
    } catch (error) {
      console.error('Error getting response:', error);
      setMessages(prev => [...prev, { 
        text: 'Sorry, I encountered an error. Please try again.',
        sender: 'bot',
        isError: true
      }]);
    } finally {
      setIsTyping(false);
    }
  };
  
  return (
    <div className="chatbot-container">
      <div className="chatbot-header">
        <h3>AI Assistant</h3>
      </div>
      
      <div className="chatbot-messages">
        {messages.length === 0 && (
          <div className="welcome-message">
            <p>Hello! How can I help you today?</p>
          </div>
        )}
        
        {messages.map((msg, index) => (
          <div 
            key={index} 
            className={`message ${msg.sender} ${msg.isError ? 'error' : ''}`}
          >
            {msg.text}
          </div>
        ))}
        
        {isTyping && (
          <div className="message bot typing">
            <span className="typing-indicator">
              <span></span>
              <span></span>
              <span></span>
            </span>
          </div>
        )}
        
        <div ref={messagesEndRef} />
      </div>
      
      <form className="chatbot-input" onSubmit={handleSubmit}>
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="Type your message..."
          disabled={isTyping}
        />
        <button type="submit" disabled={isTyping || !input.trim()}>
          Send
        </button>
      </form>
    </div>
  );
};

export default ChatbotUI;
```

#### Material-UI

A popular React UI framework implementing Google's Material Design.

**Installation:**
```bash
npm install @mui/material @emotion/react @emotion/styled
```

**Key Features:**
- Comprehensive set of pre-designed components
- Customizable theming
- Responsive design
- Accessibility support

**Code Example - Material UI Chatbot:**
```jsx
import React, { useState, useEffect, useRef } from 'react';
import { 
  Box, 
  Paper, 
  Typography, 
  TextField, 
  IconButton, 
  CircularProgress,
  Avatar,
  Divider
} from '@mui/material';
import { Send as SendIcon } from '@mui/icons-material';
import { ThemeProvider, createTheme } from '@mui/material/styles';

const theme = createTheme({
  palette: {
    primary: {
      main: '#1976d2',
    },
    secondary: {
      main: '#f50057',
    },
  },
});

const MaterialChatbot = ({ onSendMessage, initialMessages = [] }) => {
  const [messages, setMessages] = useState(initialMessages);
  const [input, setInput] = useState('');
  const [isLoading, setIsLoading] = useState(false);
  const messagesEndRef = useRef(null);
  
  useEffect(() => {
    scrollToBottom();
  }, [messages]);
  
  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  };
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    if (!input.trim() || isLoading) return;
    
    // Add user message
    const userMessage = { text: input, sender: 'user' };
    setMessages(prev => [...prev, userMessage]);
    setInput('');
    
    // Show loading
    setIsLoading(true);
    
    try {
      // Get response from parent component
      const response = await onSendMessage(input);
      
      // Add bot response
      setMessages(prev => [...prev, { text: response, sender: 'bot' }]);
    } catch (error) {
      console.error('Error getting response:', error);
      setMessages(prev => [...prev, { 
        text: 'Sorry, I encountered an error. Please try again.',
        sender: 'bot',
        isError: true
      }]);
    } finally {
      setIsLoading(false);
    }
  };
  
  return (
    <ThemeProvider theme={theme}>
      <Paper 
        elevation={3} 
        sx={{ 
          width: '100%', 
          maxWidth: 600, 
          height: 500, 
          display: 'flex', 
          flexDirection: 'column',
          mx: 'auto'
        }}
      >
        <Box sx={{ p: 2, bgcolor: 'primary.main', color: 'white' }}>
          <Typography variant="h6">AI Assistant</Typography>
        </Box>
        
        <Divider />
        
        <Box 
          sx={{ 
            flexGrow: 1, 
            p: 2, 
            overflowY: 'auto',
            display: 'flex',
            flexDirection: 'column',
            gap: 1
          }}
        >
          {messages.length === 0 && (
            <Typography color="text.secondary" align="center" sx={{ my: 2 }}>
              Hello! How can I help you today?
            </Typography>
          )}
          
          {messages.map((msg, index) => (
            <Box 
              key={index} 
              sx={{ 
                display: 'flex',
                justifyContent: msg.sender === 'user' ? 'flex-end' : 'flex-start',
                mb: 1
              }}
            >
              {msg.sender === 'bot' && (
                <Avatar 
                  sx={{ 
                    bgcolor: 'primary.main',
                    width: 32,
                    height: 32,
                    mr: 1
                  }}
                >
                  AI
                </Avatar>
              )}
              
              <Paper 
                variant="outlined"
                sx={{ 
                  p: 1.5, 
                  maxWidth: '70%',
                  bgcolor: msg.sender === 'user' ? 'primary.light' : 'grey.100',
                  color: msg.sender === 'user' ? 'white' : 'text.primary',
                  borderRadius: msg.sender === 'user' ? '16px 16px 4px 16px' : '16px 16px 16px 4px'
                }}
              >
                <Typography variant="body1">{msg.text}</Typography>
              </Paper>
              
              {msg.sender === 'user' && (
                <Avatar 
                  sx={{ 
                    bgcolor: 'secondary.main',
                    width: 32,
                    height: 32,
                    ml: 1
                  }}
                >
                  U
                </Avatar>
              )}
            </Box>
          ))}
          
          {isLoading && (
            <Box sx={{ display: 'flex', alignItems: 'center', gap: 1, ml: 1 }}>
              <Avatar 
                sx={{ 
                  bgcolor: 'primary.main',
                  width: 32,
                  height: 32
                }}
              >
                AI
              </Avatar>
              <CircularProgress size={20} />
            </Box>
          )}
          
          <div ref={messagesEndRef} />
        </Box>
        
        <Divider />
        
        <Box 
          component="form" 
          onSubmit={handleSubmit}
          sx={{ 
            p: 2, 
            display: 'flex', 
            alignItems: 'center',
            gap: 1
          }}
        >
          <TextField
            fullWidth
            size="small"
            placeholder="Type your message..."
            value={input}
            onChange={(e) => setInput(e.target.value)}
            disabled={isLoading}
            variant="outlined"
          />
          <IconButton 
            color="primary" 
            type="submit" 
            disabled={isLoading || !input.trim()}
            sx={{ p: 1 }}
          >
            {isLoading ? <CircularProgress size={24} /> : <SendIcon />}
          </IconButton>
        </Box>
      </Paper>
    </ThemeProvider>
  );
};

export default MaterialChatbot;
```

## Installation and Setup Instructions

### Setting Up a .NET C# Project

1. **Install .NET SDK**:
   - Download and install from https://dotnet.microsoft.com/download

2. **Create a New Project**:
   ```bash
   dotnet new webapi -n ChatbotApi
   cd ChatbotApi
   ```

3. **Add Required Packages**:
   ```bash
   dotnet add package OpenAI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.AspNetCore.SignalR
   ```

4. **Run the Project**:
   ```bash
   dotnet run
   ```

### Setting Up a Node.js Project

1. **Install Node.js**:
   - Download and install from https://nodejs.org/

2. **Create a New Project**:
   ```bash
   mkdir chatbot-app
   cd chatbot-app
   npm init -y
   ```

3. **Add Required Packages**:
   ```bash
   npm install express openai langchain mongodb socket.io cors
   npm install --save-dev nodemon
   ```

4. **Add Start Script to package.json**:
   ```json
   "scripts": {
     "start": "node server.js",
     "dev": "nodemon server.js"
   }
   ```

5. **Run the Project**:
   ```bash
   npm run dev
   ```

### Setting Up a React Frontend

1. **Create a New React App**:
   ```bash
   npx create-react-app chatbot-frontend
   cd chatbot-frontend
   ```

2. **Add Required Packages**:
   ```bash
   npm install axios socket.io-client @mui/material @emotion/react @emotion/styled @mui/icons-material
   ```

3. **Run the Frontend**:
   ```bash
   npm start
   ```

## Code Examples and Integration

The code examples provided throughout this section demonstrate how to use these libraries and frameworks to build AI chatbots. To integrate them into a complete solution:

1. **Backend API**: Use ASP.NET Core or Express.js to create the backend API that handles chatbot requests.

2. **LLM Integration**: Connect to OpenAI, Anthropic, or other LLM providers using the appropriate client libraries.

3. **Vector Database**: Implement a vector database for efficient retrieval of relevant information.

4. **Frontend UI**: Create a user-friendly chat interface using React or another frontend framework.

5. **Real-time Communication**: Use SignalR or Socket.IO for real-time chat functionality.

6. **Data Storage**: Implement conversation storage using Entity Framework, MongoDB, or another database solution.

By combining these components, you can create sophisticated AI chatbot applications that meet your specific business requirements.

The libraries and frameworks listed in this section provide all the tools you need to implement the chatbot solutions described in the previous sections, without requiring deep expertise in AI or machine learning.
