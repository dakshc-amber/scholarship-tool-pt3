export default async function handler(req, res) {
  if (req.method === "OPTIONS") {
    return res.status(200).end();
  }

  if (req.method !== "POST") {
    return res.status(405).json({
      error: {
        message: "Method Not Allowed"
      }
    });
  }

  if (!process.env.ANTHROPIC_API_KEY) {
    return res.status(500).json({
      error: {
        message: "ANTHROPIC_API_KEY is not configured."
      }
    });
  }

  try {
    const response = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "x-api-key": process.env.ANTHROPIC_API_KEY,
        "anthropic-version": "2023-06-01"
      },
      body: JSON.stringify(req.body)
    });

    const text = await response.text();

    let data;

    try {
      data = JSON.parse(text);
    } catch {
      return res.status(response.status).json({
        error: {
          message: text
        }
      });
    }

    return res.status(response.status).json(data);

  } catch (error) {
    console.error(error);

    return res.status(500).json({
      error: {
        message: error.message || "Internal Server Error"
      }
    });
  }
}
