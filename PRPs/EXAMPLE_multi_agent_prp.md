name: "Base PRP Template v2 - Context-Rich with Validation Loops"description: |PurposeTemplate optimized for AI agents to implement features with sufficient context and self-validation capabilities to achieve working code through iterative refinement.Core PrinciplesContext is King: Include ALL necessary documentation, examples, and caveatsValidation Loops: Provide executable tests/lints the AI can run and fixInformation Dense: Use keywords and patterns from the codebaseProgressive Success: Start simple, validate, then enhanceGlobal rules: Be sure to follow all rules in node-ts-guidelines.mdGoal[What needs to be built - be specific about the end state and desires]Why[Business value and user impact][Integration with existing features][Problems this solves and for whom]What[User-visible behavior and technical requirements]Success Criteria[ ] [Specific measurable outcomes]All Needed ContextDocumentation & References (list all context needed to implement the feature)# MUST READ - Include these in your context window
- url: [Official API docs URL]
  why: [Specific sections/methods you'll need]
  
- file: [path/to/example.ts]
  why: [Pattern to follow, gotchas to avoid]
  
- doc: [Library documentation URL] 
  section: [Specific section about common pitfalls]
  critical: [Key insight that prevents common errors]

- docfile: [PRPs/ai_docs/file.md]
  why: [docs that the user has pasted in to the project]

Current Codebase tree (run tree in the root of the project) to get an overview of the codebase
Desired Codebase tree with files to be added and responsibility of file
Known Gotchas of our codebase & Library Quirks// CRITICAL: [Library name] requires [specific setup]
// Example: Express.js middleware order is crucial
// Example: This ORM doesn't support batch inserts over 1000 records
// Example: We use Zod for schema validation and custom error handling
Implementation BlueprintData models and structureCreate the core data models, we ensure type safety and consistency.// Examples: 
// - TypeScript interfaces/types
// - Zod schemas
// - Joi schemas
// - Database ORM models (e.g., TypeORM entities, Prisma schemas)

interface User {
  id: string;
  name: string;
  email: string;
}

const UserSchema = z.object({
  id: z.string().uuid(),
  name: z.string().min(3),
  email: z.string().email(),
});
list of tasks to be completed to fulfill the PRP in the order they should be completedTask 1:
MODIFY src/existingModule.ts:
  - FIND pattern: "class OldImplementation"
  - INJECT after line containing "constructor("
  - PRESERVE existing method signatures

CREATE src/newFeature.ts:
  - MIRROR pattern from: src/similarFeature.ts
  - MODIFY class name and core logic
  - KEEP error handling pattern identical

...(...)

Task N:
...

Per task pseudocode as needed added to each task
// Task 1
// Pseudocode with CRITICAL details, don't write entire code
async function newFeature(param: string): Promise<Result> {
  // PATTERN: Always validate input first (see src/validators/index.ts)
  const validated = validateInput(param); // throws ZodError or custom ValidationError
  
  // GOTCHA: This library requires connection pooling
  const conn = await getConnection(); // see src/db/connection.ts
  try {
    // PATTERN: Use existing retry decorator/utility
    const result = await retry(async () => {
      // CRITICAL: API returns 429 if >10 req/sec
      await rateLimiter.acquire();
      return await externalApi.call(validated);
    }, { attempts: 3, backoff: 'exponential' });
    
    // PATTERN: Standardized response format
    return formatResponse(result); // see src/utils/responses.ts
  } finally {
    conn.release(); // Ensure connection is released
  }
}
Integration PointsDATABASE:
  - migration: "Add column 'feature_enabled' to users table"
  - index: "CREATE INDEX idx_feature_lookup ON users(feature_id)"
  
CONFIG:
  - add to: src/config/index.ts
  - pattern: "export const FEATURE_TIMEOUT = parseInt(process.env.FEATURE_TIMEOUT || '30', 10);"
  
ROUTES:
  - add to: src/api/routes/index.ts (or src/app.ts)
  - pattern: "app.use('/feature', featureRouter);"
Validation LoopLevel 1: Syntax & Style# Run these FIRST - fix any errors before proceeding
npm run lint -- --fix src/newFeature.ts # Auto-fix what's possible (uses ESLint)
npm run typecheck src/newFeature.ts     # Type checking (uses tsc --noEmit)

# Expected: No errors. If errors, READ the error and fix.
Level 2: Unit Tests each new feature/file/function use existing test patterns// CREATE tests/newFeature.test.ts with these test cases:
import { newFeature } from '../src/newFeature';
import { ZodError } from 'zod'; // Or your specific validation error

describe('newFeature', () => {
  test('should return success for valid input', async () => {
    const result = await newFeature('valid_input');
    expect(result.status).toBe('success');
  });

  test('should throw validation error for invalid input', async () => {
    await expect(newFeature('')).rejects.toThrow(ZodError); // or specific validation error
  });

  test('should handle external API timeout gracefully', async () => {
    jest.spyOn(externalApi, 'call').mockRejectedValueOnce(new Error('TimeoutError'));
    const result = await newFeature('valid');
    expect(result.status).toBe('error');
    expect(result.message).toContain('timeout');
  });
});
```bash
# Run and iterate until passing:
npm test tests/newFeature.test.ts
# If failing: Read error, understand root cause, fix code, re-run (never mock to pass)
Level 3: Integration Test# Start the service
npm start # Or `node dist/main.js` if you have a build step

# Test the endpoint
curl -X POST http://localhost:8000/feature \
  -H "Content-Type: application/json" \
  -d '{"param": "test_value"}'

# Expected: {"status": "success", "data": {...}}
# If error: Check logs at logs/app.log (or your configured log path) for stack trace
Final validation Checklist[ ] All tests pass: npm test[ ] No linting errors: npm run lint[ ] No type errors: npm run typecheck[ ] Manual test successful: [specific curl/command][ ] Error cases handled gracefully[ ] Logs are informative but not verbose[ ] Documentation updated if neededAnti-Patterns to Avoid❌ Don't create new patterns when existing ones work❌ Don't skip validation because "it should work"❌ Don't ignore failing tests - fix them❌ Don't use blocking operations in async context❌ Don't hardcode values that should be config❌ Don't catch all exceptions - be specific
