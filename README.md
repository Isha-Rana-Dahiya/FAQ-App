# FAQ-App
# Friendly-FAQ-Chatbot-App
**Step 1:** Project Setup
Create a new project:
- Template: Empty Compose Activity
- Name: Friendly FAQ Chatbot App
- Minimum SDK: API 24("Nougat";Android 7.0)

<img width="939" height="494" alt="image" src="https://github.com/user-attachments/assets/532f4f6f-365c-4c34-a8d9-98758c50dfb1" />


**Step 2:** Define Data Model
Create a simple model to represent FAQs grouped by tags.
Kotlin
data class FAQ(
    val keyword: String,
    val question: String,
    val answer: String
)

**Step 3:** Create Static FAQ Dataset
You’ll use a Kotlin list or map to store predefined FAQs:
Kotlin
val faqData = listOf(
    FAQ("Cancel", "Can I cancel my order?", "Before shipment : Yes - Please email to ask@abc.com to request for cancellation. After shipment : You'll need to follow the standard return/refund process once the seller marks it as shipped."),
    FAQ("Refund", "How do I request a refund?", "Go to your membership account and open the order. Click “Request Refund” within 7 days of delivery (or within 10 days after the expected delivery date if undelivered). Submit any supporting evidence (photos, courier updates). The seller has 3 business days to respond."),
    FAQ("Track", "How can I track my shipment?", "After your order ships, the seller must upload a valid tracking number to your order detail page. You’ll also receive an email notification with the tracking link.")
)

Or for keyword grouping:
Kotlin
val faqMap = mapOf(
    "Returns" to listOf("Can I return a product?" to "Yes, returns are accepted within 7 days."),
    "Pricing" to listOf("Is tax included?" to "Prices are inclusive of GST.")
)

💬 Step 4: Chat UI with Jetpack Compose
Create a layout that mimics basic chat:
Kotlin
@Composable
fun FAQChatScreen() {
    var userQuery by remember { mutableStateOf("") }
    var botResponse by remember { mutableStateOf("") }

    Column(modifier = Modifier.padding(16.dp)) {
        TextField(
            value = userQuery,
            onValueChange = { userQuery = it },
            label = { Text("Ask your question") }
        )

        Spacer(modifier = Modifier.height(8.dp))

        Button(onClick = {
            botResponse = matchFAQ(userQuery)
        }) {
            Text("Submit")
        }

        Spacer(modifier = Modifier.height(16.dp))
        Text("Bot Response: $botResponse")
    }
}

🔎 Step 5: Matching Logic for FAQs
Implement simple keyword or substring matching:
Kotlin
fun matchFAQ(query: String): String {
    val lowerQuery = query.lowercase()
    for (faq in faqData) {
        if (lowerQuery.contains(faq.keyword.lowercase()) ||
            lowerQuery.contains(faq.question.lowercase())) {
            return faq.answer
        }
    }
    return "Sorry, I don't have an answer for that yet."
}
