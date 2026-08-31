[CHANGES.md](https://github.com/user-attachments/files/31636031/CHANGES.md)
# Engagement Letter Updates

## Changes Made

### 1. Currency Flexibility

**Problem:** Currency was hardcoded to USD with no way to change it.

**Solution:** 
- Added a new "Currency Code" field in the Advisor Setup panel
- Defaults to USD if not specified
- Stored in the client configuration and persists across links
- All fee displays now use the selected currency

**Implementation:**
- Modified `fmt()` function to call `getCurrency()` dynamically
- Added currency field input in admin panel (line 193)
- Updated `generateLink()` to include `currencyCode` in saved config
- Updated config loading to restore currency when client accesses link

**Usage:** Enter any ISO currency code (USD, AED, GBP, EUR, etc.) in the Currency Code field during advisor setup.

---

### 2. Signature Display Fix

**Problem:** Signatures were not displayed in the HTML sent to clients because embedded signature data URLs were being stripped from the engagement letter.

**Solution:**
- Modified `send-engagement.js` function to preserve data URLs
- Previously, the function was removing all embedded image data (line 29)
- This was intended to reduce file size, but also removed the signature

**Implementation:**
- Removed the line that stripped data URL images: `decoded = decoded.replace(/src="data:image\/[^"]+"/g, 'src=""');`
- Signatures (drawn on canvas) are now preserved in the sent HTML
- D4's stamped signature image continues to display correctly

**Result:** When a client submits a signed engagement letter, their signature is now clearly visible in the received HTML document.

---

## Files Modified

1. **index.html**
   - Added currency input field to advisor panel
   - Updated `fmt()` function for dynamic currency
   - Modified `generateLink()` to save currency
   - Updated config loading to restore currency

2. **netlify/functions/send-engagement.js**
   - Removed data URL stripping logic
   - Signatures now persist through email transmission

---

## Testing

1. **Currency Test:**
   - Open Advisor Setup
   - Enter "AED" or another currency code
   - Confirm all fees display with selected currency
   - Generate client link and verify currency persists

2. **Signature Test:**
   - Fill out engagement letter
   - Draw signature in the signature canvas
   - Submit engagement letter
   - Verify signature appears in received HTML

---

## Default Behavior

- If no currency is specified: defaults to USD
- If advisor setup is not used: USD is the default
- Currency selection is stored per client link
