import re
from fastapi import FastAPI, HTTPException, Request
from pydantic import BaseModel

app = FastAPI(title="Hardened LLM Gateway AI Firewall")

# 🛡️ Defensive Heuristics Array (Mitigating LLM01: Prompt Injection)
PROMPT_INJECTION_BLACKLIST = [
    r"(?i)\bignore previous instructions\b",
    r"(?i)\bsystem prompt\b",
    r"(?i)\byou are now an admin\b",
    r"(?i)\boutput the raw flag\b",
    r"(?i)\bdisregard safety guidelines\b",
    r"(?i)\boverride policy\b"
]

# 🔒 Data Loss Prevention Patterns (Mitigating LLM06: Sensitive Data Disclosure)
# Matches generic AWS Access Keys and common corporate internal local domains (.corp.local)
DLP_BLACKLIST = [
    r"AKIA[0-9A-Z]{16}", 
    r"[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.corp\.local"
]

class LLMRequest(BaseModel):
    user_prompt: str

class LLMResponse(BaseModel):
    safe_output: str

def inspect_input(prompt: str) -> bool:
    """Scans incoming user prompts against known prompt injection heuristics."""
    for pattern in PROMPT_INJECTION_BLACKLIST:
        if re.search(pattern, prompt):
            return True
    return False

def sanitize_output(response_text: str) -> str:
    """Scans and redacts sensitive internal data patterns before outputting to user."""
    sanitized = response_text
    for pattern in DLP_BLACKLIST:
        sanitized = re.sub(pattern, "[REDACTED_SENSITIVE_DATA]", sanitized)
    return sanitized

@app.post("/v1/chat/secure", response_model=LLMResponse)
async def secure_llm_proxy(payload: LLMRequest):
    # 1. Inbound Inspection (Shift-Left Input Validation)
    if inspect_input(payload.user_prompt):
        raise HTTPException(
            status_code=403, 
            detail="Security Violation: Input blocked by AI Firewall policy due to potential prompt injection."
        )
    
    # 2. Simulated LLM Processing Backend Loop
    # (In production, this would make an authenticated async call to your backend model API)
    simulated_model_raw_output = (
        f"Processed prompt successfully. Internal log reference code: "
        f"AKIAIOSFODNN7EXAMPLE generated on auth-server.corp.local"
    )
    
    # 3. Outbound Data Loss Prevention Sanitization
    clean_output = sanitize_output(simulated_model_raw_output)
    
    return LLMResponse(safe_output=clean_output)

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8080)
