
using namespace std;

// Token types
enum TokenType {  IDENTIFIER,KEYWORD,OPERATOR,NUMBER,STRING,EOF)
};

// Token structure
struct Token {
    TokenType type;
    string value;
};

// Keywords
const map<string, TokenType> keywords = {
    {"int", KEYWORD},
    {"float", KEYWORD},
    {"char", KEYWORD},
    {"if", KEYWORD},
    {"else", KEYWORD},
    {"while", KEYWORD},
    {"return", KEYWORD},
    {"print", KEYWORD},
};

// Operators
const vector<string> operators = {
    "+", "-", "*", "/", "%", "=", "==", "!=", "<", ">", "<=", ">=", "&&", "||", "!", "++", "--"
};

// Lexer
vector<Token> lex(const string& sourceCode) {
    vector<Token> tokens;
    int i = 0;
    while (i < sourceCode.length()) {
        char currentChar = sourceCode[i];

        // Skip whitespace
        if (isspace(currentChar)) {
            i++;
            continue;
        }

        // Identifier or keyword
        if (isalpha(currentChar) || currentChar == '_') {
            string identifier = "";
            while (isalnum(currentChar) || currentChar == '_') {
                identifier += currentChar;
                i++;
                currentChar = sourceCode[i];
            }

            if (keywords.count(identifier) > 0) {
                tokens.push_back({keywords.at(identifier), identifier});
            } else {
                tokens.push_back({IDENTIFIER, identifier});
            }
        }

        // Number
        if (isdigit(currentChar)) {
            string number = "";
            while (isdigit(currentChar)) {
                number += currentChar;
                i++;
                currentChar = sourceCode[i];
            }
            tokens.push_back({NUMBER, number});
        }

        // String
        if (currentChar == '"') {
            string str = "";
            i++;
            currentChar = sourceCode[i];
            while (currentChar != '"') {
                str += currentChar;
                i++;
                currentChar = sourceCode[i];
            }
            i++;
            tokens.push_back({STRING, str});
        }

        // Operator
        for (const string& op : operators) {
            if (sourceCode.substr(i, op.length()) == op) {
                tokens.push_back({OPERATOR, op});
                i += op.length();
                break;
            }
        }
    }

    // Add EOF token
    tokens.push_back({EOF, ""});
    return tokens;
}

// Parser (Simplified)
void parse(const vector<Token>& tokens) {
    int i = 0;
    while (tokens[i].type != EOF) {
        // ... parsing logic ...
        // Check token type and perform appropriate actions
        i++;
    }
}

int main() {
    // Read source code from file
    ifstream inputFile("source.txt");
    string sourceCode((istreambuf_iterator<char>(inputFile)), istreambuf_iterator<char>());

    // Lexical analysis
    vector<Token> tokens = lex(sourceCode);

    // Print tokens (for debugging)
    cout << "Tokens:" << endl;
    for (const Token& token : tokens) {
        cout << "Type: " << token.type << ", Value: " << token.value << endl;
    }

    // Parsing
    parse(tokens);

    return 0;
}
